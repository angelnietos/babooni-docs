<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Runbook — Backup y restore de PII cifrada</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

> Operativo. Contexto de cifrado y `kid`: ADR 0003. Volver al [docs hub](../../README.md).

Este runbook cubre el respaldo y la restauración de los datos sensibles (PII)
del backend, que se almacenan **cifrados con AES-256-GCM** (`FieldEncryptionService`)
bajo una clave activa identificada por `kid` (`KeyProvider` / `EnvMasterKeyProvider`).

> ⚠️ **La clave es parte del dato.** Un backup de la base de datos sin las claves
> (`APP_ENCRYPTION_KEYS`, `APP_ENCRYPTION_KEY_ID`) es **ilegible**. Guarda siempre
> el backup de datos y el backup de claves de forma separada y con distinto
> control de acceso.

> 🔑 **Claves de cifrado (AES-256-GCM).** Cada sobre cifrado lleva su propio `kid`
> (key id). El `KeyProvider` resuelve la clave por `kid`, así que sobres viejos
> siguen descifrables tras rotar. La rotación en masa se hace con
> `KeyRotationService.rotateModel` (ver §4), no hay endpoint HTTP para ello.

## 1. Backup (dump cifrado)

```bash
# 1.1 Volcado de la base (Postgres) — single-tenant
pg_dump "$DATABASE_URL" \
  | gzip \
  > "backup-$(date +%Y%m%d-%H%M%S).sql.gz"

# 1.2 Cifrado del dump en reposo (clave de backup distinta a la de campo)
gpg --batch --yes --symmetric --cipher-algo AES256 \
  --output "backup-$(date +%Y%m%d-%H%M%S).sql.gz.gpg" \
  "backup-$(date +%Y%m%d-%H%M%S).sql.gz"

# 1.3 Subir a almacenamiento seguro (ej. S3 con versionado + bloqueo)
aws s3 cp "backup-$(date +%Y%m%d-%H%M%S).sql.gz.gpg" \
  "s3://arquetipos-backups/pii/"

# 1.4 Limpiar local
rm "backup-$(date +%Y%m%d-%H%M%S).sql.gz" "backup-$(date +%Y%m%d-%H%M%S).sql.gz.gpg"
```

## 2. Backup de claves (CRÍTICO)

```bash
# Exportar SOLO en un secreto gestionado (Vault / KMS / sealed-secret).
# NUNCA en el mismo bucket que el dump de datos.
vault kv put secret/arquetipos/encryption \
  keys="$APP_ENCRYPTION_KEYS" \
  key_id="$APP_ENCRYPTION_KEY_ID"
```

## 3. Restore

```bash
# 3.1 Descargar y descifrar el dump
aws s3 cp "s3://arquetipos-backups/pii/$BACKUP" ./
gpg --decrypt "$BACKUP" > restore.sql.gz
gunzip restore.sql.gz

# 3.2 Restaurar esquema + datos
psql "$DATABASE_URL" -f restore.sql

# 3.3 Restaurar las claves ANTES de arrancar la API
export APP_ENCRYPTION_KEYS="$(vault kv get -field=keys secret/arquetipos/encryption)"
export APP_ENCRYPTION_KEY_ID="$(vault kv get -field=key_id secret/arquetipos/encryption)"
```

## 4. Rotación de claves (KMS / `KeyRotationService`)

Tras exponer/perder una clave, sigue el runbook corto
[`pii-key-rotation.md`](./pii-key-rotation.md): añade la nueva a
`APP_ENCRYPTION_KEYS`, eleva `APP_ENCRYPTION_KEY_ID`, y re-cifra en batches.
Los sobres existentes siguen descifrables (llevan su propio `kid`). Resumen
operativo:

`rotate-pii` es un **job de BullMQ** (`JobType.RotatePii`), no un endpoint HTTP.
Se encola vía el servicio de jobs (requiere `REDIS_HOST`; sin él, `JobsModule`
degrada a `NoopJobsService` y el job no se ejecuta):

```ts
// En cualquier módulo NestJS (el JobsModule es @Global):
import { Inject, Logger } from '@nestjs/common';
import { JOBS_SERVICE, JobType, JobPayload } from '@base/backend'; // ajusta el import al barrel del producto

@Injectable()
class RotatePiiInvoker {
  constructor(@Inject(JOBS_SERVICE) private readonly jobs: { enqueue: (j: JobPayload) => Promise<void> }) {}

  async rotate() {
    await this.jobs.enqueue({ type: JobType.RotatePii, payload: {} });
  }
}
```

El `JobsProcessor` despacha el job a `KeyRotationService.rotateModel` para cada
modelo con PII. También puede invocarse directamente (fuera de BullMQ) para un
tenant/volumen concreto:

```bash
# Ejemplo operativo (fuera del alcance del HTTP): ejecuta un script NestJS que
# llame a KeyRotationService.rotateAll(), o espera a que el worker drene la cola.
```

Verifica en `/api/health/ready` y que `GET /api/clients` sigue devolviendo PII
legible tras la rotación.

## 5. Verificación post-restore

- `GET /api/health/ready` → 200 (DB accesible).
- `GET /api/clients` con un tenant conocido → registros con PII legible (descifrado OK).
- Comprobar en un registro que `kid` activo coincide con `APP_ENCRYPTION_KEY_ID`.

## 6. RTO / RPO objetivo

| Métrica | Objetivo |
|---------|----------|
| RPO     | ≤ 24 h (dump diario cifrado) |
| RTO     | ≤ 4 h (restore + descifrado + arranque) |
