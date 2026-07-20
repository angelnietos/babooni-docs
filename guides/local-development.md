# Desarrollo local — referencia

Complemento de [getting-started.md](../getting-started.md) con detalle operativo.

---

## Servicios y puertos (dev)

| Servicio | Puerto host | Variable / notas |
|----------|-------------|------------------|
| Postgres | 5440 | `DATABASE_URL` |
| Redis | 6381 | `REDIS_HOST`, `REDIS_PORT` — opcional |
| Kafka | 9094 | `KAFKA_BROKERS` |
| Keycloak | 9080 | `KEYCLOAK_URL` |
| josanz-api | 3000 | `pnpm josanz-api:dev` |
| josanz SPA | 4200 | `pnpm nx serve josanz` |
| api (monolito) | 3000 | `pnpm nx serve api` |
| api-gateway | 4000 | `GATEWAY_PORT` |
| clients-ms gRPC | 50051 | `CLIENTS_MS_GRPC_PORT` |
| clients-ms HTTP | 4001 | health |

Catálogo completo: [SERVICES.md](../../SERVICES.md).

---

## Variables de entorno por app

Copia `.env.example` → `.env`. Cada app puede tener env **específica** que se normaliza a `DATABASE_URL` en su bootstrap:

| App | Env dedicada (prioridad) | Bootstrap |
|-----|--------------------------|-----------|
| `josanz-api` | `JOSANZ_DATABASE_URL` | `apps/clientes/josanz/backend/src/bootstrap-env.ts` |
| `api-single` | `ARQUETIPOS_DATABASE_URL` | `apps/arquetipos/backend/monolith/api-single/src/bootstrap-env.ts` |
| `clients-ms` | `CLIENTS_MS_DATABASE_URL` | `apps/arquetipos/backend/microservices/clients-ms/src/bootstrap-env.ts` |
| `verifactu-crm-api` | `VERIFACTU_DATABASE_URL` | `resolveCrmDatabaseUrl()` |

Si no defines la env dedicada, se usa `DATABASE_URL` del `.env` raíz (**BD compartida**).

---

## Comandos frecuentes

```bash
# Infra
pnpm dev:infra

# Josanz — migrate + seed
pnpm josanz:bootstrap
pnpm josanz:seed-demo          # re-seed idempotente

# Prisma
pnpm migrate:deploy:single    # schema single-tenant (Josanz)
pnpm migrate:deploy           # schema multi-tenant
pnpm prisma:generate

# CRM SaaS
pnpm saas:crm:migrate
pnpm saas:crm:generate

# Tests
npx jest --config libs/base/backend/jest.config.ts
npx jest --config apps/clientes/josanz/backend/jest.config.ts --runInBand

# Convenciones
node tools/scripts/check-lib-layout.mjs --strict
pnpm check:legacy-paths
```

---

## Keycloak — realms

| Producto / plantilla | Realm | Client id típico |
|----------------------|-------|------------------|
| Josanz | `josanz` | `josanz-web` — forzado en bootstrap `josanz-api` |
| Arquetipos | `arquetipos` | `arquetipos-web` |

Si el SPA usa realm `josanz` pero el API valida `arquetipos`, verás 401. Alinea `.env` o confía en el bootstrap de producto.

---

## Resiliencia local (no romper al desarrollar)

- Sin **Redis** → jobs BullMQ no-op; idempotencia in-memory.
- Sin **Kafka** → outbox relay reintenta; event bus in-memory en monolito.
- Sin **Postgres** → health `503`; integration tests se auto-skip.

Un cambio nuevo debe preservar «arranca sin infra opcional».

---

## Problemas habituales

| Síntoma | Causa | Solución |
|---------|-------|----------|
| `nx serve` cuelga | Daemon Nx | Usar `npx tsc -p … --noEmit` o `pnpm josanz-api:dev` |
| 401 en Josanz | Realm Keycloak | `KEYCLOAK_REALM=josanz` en API |
| Sin datos en dashboard | Sin seed | `pnpm josanz:bootstrap` |
| Errores TS en `*-features/*` paths | Artefacto IDE | Wildcards en `tsconfig.base.json` — `tsc` sí pasa |
| `pnpm install` falla workspace | Falta `workspace:*` | `pnpm add @base/foo --filter @josanz/bar --workspace` |

---

## Verifactu mínimo (perfil postgres + redis)

Para smokes de cola y demo del CRM Verifactu **no** hace falta Keycloak ni Kafka. Basta:

```bash
# 1. Infra mínima (sin Keycloak/Kafka)
pnpm dev:infra:minimal        # docker compose up -d postgres redis
pnpm saas:infra:preflight     # verifica docker + postgres:5440 + redis:6381

# 2. CRM DB (one-shot) + prisma + seed
docker exec $(docker compose -f docker-compose.dev.yml ps -q postgres) \
  psql -U arquetipos -d arquetipos -c "CREATE DATABASE generic_crm;"
DATABASE_URL="postgresql://arquetipos:arquetipos@localhost:5440/generic_crm?schema=public" \
  pnpm saas:crm:generate && pnpm saas:crm:migrate && pnpm saas:crm:seed

# 3. Smokes reproducibles
pnpm saas:worker:smoke        # Redis :6381 → exit 0
VERIFACTU_SMOKE_API=1 pnpm saas:verifactu:smoke   # API+worker+Redis+DB
pnpm test:smoke:verifactu-platform     # UI :4230
```

Runbook completo: [docs/runbooks/verifactu-demo-e2e.md](../runbooks/verifactu-demo-e2e.md).
Puertos: Postgres `5440`, Redis `6381`, CRM API `3120`, Worker `3130`, Platform `4230`.

## Enlaces

- [database-migrations.md](../runbooks/database-migrations.md)
- [pnpm-layout.md](../runbooks/pnpm-layout.md)
- [kafka-redis-outage.md](../runbooks/kafka-redis-outage.md)
