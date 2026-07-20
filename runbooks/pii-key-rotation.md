# Runbook — PII key rotation

> Operativo. Contexto de cifrado: [ADR 0003](../adr/adr-0003-aes-256-gcm-vs-kms.md).  
> Backup/restore completo: [backup-restore-pii.md](./backup-restore-pii.md).

Rotación **online** de claves AES-256-GCM usadas por `FieldEncryptionService`.
Cada sobre lleva su propio `kid`; no borres una clave antigua hasta que el %
migrado sea ~100 % y los backups que la necesitan hayan expirado.

## Preflight

1. Añade la clave nueva a `APP_ENCRYPTION_KEYS` (JSON `kid -> hex/base64`).
2. Mantén la(s) clave(s) vieja(s) en el mismo mapa.
3. Eleva `APP_ENCRYPTION_KEY_ID` a la clave nueva (escrituras nuevas la usan).
4. Redeploy / restart API + worker para cargar el mapa.

## Ejecutar rotación

Encola el job BullMQ `rotate-pii` (`JobType.RotatePii`) o llama
`KeyRotationService.rotateAll()` / `rotateModel(model, batchSize)` desde un
script Nest.

- `batchSize` por defecto `200` — baja el tamaño si hay locks / timeouts.
- El servicio **no aborta el job** ante un fallo de fila: reporta
  `{ rotated, skipped, failed, errors }` y continúa.
- Filas fallidas siguen legibles con el `kid` antiguo (mixed keyIds OK).

## Verificar

1. `GET /api/health/ready` → 200.
2. Leer un registro PII cifrado antes de la rotación → plaintext OK.
3. Tras rotar, muestrear filas: la mayoría con `kid` = `APP_ENCRYPTION_KEY_ID`.
4. Si `failed > 0`, re-ejecuta el job; no borres claves viejas aún.

## Retención de claves viejas

| Fase | Acción |
|------|--------|
| Durante migración | Conservar todas las keys del mapa |
| Migración ~100 % + re-run limpio | Conservar keys viejas ≥ retención de backups (p. ej. 30–90 días) |
| Tras expirar backups que usan kid viejo | Quitar kid del mapa y rotar secretos en Vault/KMS |

## No hacer

- No borrar `APP_ENCRYPTION_KEYS` entries mid-batch.
- No loguear ciphertext ni material de clave (el rotator solo loguea ids/errores).
- No exponer un endpoint HTTP público de rotación — solo job/script interno.
