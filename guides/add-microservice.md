<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Extraer un microservicio</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cómo pasar de **monolito** a **proceso distribuido** reutilizando las mismas libs.

---

## Cuándo tiene sentido

- Escalar un dominio concreto (p. ej. `clients`) de forma independiente.
- Aislar fallos o desplegar cadencias distintas.
- **No** es obligatorio — el monolito modular (`josanz-api`, `api`) es el camino por defecto.

---

## Principio

> Misma lib de dominio, distinto **composition root** y distinto **transporte**.

`clients-ms` importa el mismo `ClientsModule` que `api`, pero:

- Expone **gRPC** (`ClientsGrpcController`) en lugar de HTTP in-process.
- Publica eventos vía **Kafka** (`MESSAGING_DRIVER=kafka`).
- Puede usar **BD dedicada** o compartida (`CLIENTS_MS_DATABASE_URL`).

Referencia: `apps/arquetipos/backend/microservices/clients-ms/`.

---

## Pasos para `{domain}-ms`

### 1. App Nest mínima

```
apps/.../microservices/{domain}-ms/
├── src/
│   ├── main.ts
│   ├── bootstrap-env.ts
│   ├── environments.ts
│   └── app/
│       ├── app.module.ts      # solo módulos necesarios
│       ├── {domain}.grpc.controller.ts
│       └── health.controller.ts
└── project.json
```

### 2. bootstrap-env.ts

```typescript
import { applyProductDatabaseUrl } from '@base/backend';

applyProductDatabaseUrl({
  keys: ['{DOMAIN}_MS_DATABASE_URL', 'DATABASE_URL'],
  label: '{domain}-ms',
});
```

### 3. main.ts — orden de imports

```typescript
import 'dotenv/config';
import './bootstrap-env';
import './environments';
// … NestFactory, connectMicroservice, listen
```

### 4. AppModule

Importa solo:

- `MultiTenantModule` (si multi-tenant)
- `{Domain}Module` de `@base/backend` o producto
- `ObservabilityModule`, `JobsModule` según necesidad
- **No** importes dominios que no expone este servicio

### 5. Gateway (opcional)

`api-gateway` enruta `/api/gateway/clients/*` → gRPC `clients-ms`. Otros paths → proxy al monolito.

### 6. Topología de BD

| Opción | Config |
|--------|--------|
| Compartida con monolito | Solo `DATABASE_URL` (misma en ambos pods) |
| Dedicada | `CLIENTS_MS_DATABASE_URL=postgresql://…/clients_db` |

Migrar schema necesario en esa BD. Matriz: [backend-domain-convention.md § BD](../backend/backend-domain-convention.md#base-de-datos-por-app-contrato).

### 7. Documentar

Añade fila en:

- `docs/backend/backend-domain-convention.md`
- `docs/runbooks/database-migrations.md`
- `.env.example`

---

## Eventos entre servicios

- Productor: `OutboxEventBus` → Kafka (ADR 0004).
- Consumidor: idempotente por `eventId`.
- Monolito local: `MESSAGING_DRIVER=inmemory` (default en dev sin Kafka).

---

## Verificación

```bash
pnpm nx serve clients-ms
pnpm nx serve api-gateway
# gRPC health :4001, gateway :4000
```

---

## Enlaces

- [add-backend-domain.md](./add-backend-domain.md)
- [adr-0004](../adr/adr-0004-kafka-outbox.md)
- [kafka-redis-outage.md](../runbooks/kafka-redis-outage.md)
