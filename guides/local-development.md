# Desarrollo local — referencia

Complemento de [getting-started.md](../getting-started.md) con detalle operativo.

---

## Servicios y puertos (dev)

### Infra

| Servicio | Puerto host | Variable / notas |
|----------|-------------|------------------|
| Postgres | 5440 | `DATABASE_URL` |
| Redis | 6381 | `REDIS_HOST`, `REDIS_PORT` — opcional |
| Kafka | 9094 | `KAFKA_BROKERS` |
| Keycloak | 9080 | `KEYCLOAK_URL` |

### APIs y gateways

| App | Puerto | Notas |
|-----|--------|-------|
| josanz-api | 3000 | `pnpm josanz-api:dev` |
| api (monolito arquetipos) | 3000 | `pnpm nx serve api` |
| api-gateway | 4000 | `GATEWAY_PORT` |
| clients-ms gRPC | 50051 | `CLIENTS_MS_GRPC_PORT` |
| clients-ms HTTP | 4001 | health |
| verifactu-crm-api | 3120 | SaaS CRM |
| verifactu-worker | 3130 | BullMQ worker |

### Frontends web

| App | Puerto | Notas |
|-----|--------|-------|
| josanz SPA | 4200 | `pnpm nx serve josanz` |
| angular-single / multi | 4200 / 4203 | plantillas |
| react-single / multi | 4201 / 4202 | plantillas |
| next-single | 4240 | Next opt-in |
| verifactu-platform | 4230 | CRM UI |
| document-generator | 4210 | docs UI (también zona MF) |
| MF host / remotes | 4210–4212 | ver project.json |

### Mobile

| App | Puerto web | Comando |
|-----|------------|---------|
| ionic-single | 4300 | `pnpm nx serve ionic-single` |
| ionic-multi | 4301 | `pnpm nx serve ionic-multi` |
| react-native-single | 8091 | Expo `--web` |
| react-native-multi | 8092 | Expo `--web` |

Guías: [add-mobile-domain.md](./add-mobile-domain.md), [add-next-domain.md](./add-next-domain.md), [module-federation-dev.md](./module-federation-dev.md).

Catálogo HTTP: [SERVICES.md](../../SERVICES.md).

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
pnpm test:coverage:backend   # F56 — coverage libs/apps tag:runtime:backend

# Arquetipos FE (F56)
pnpm arq:fe:build:smoke      # build angular-single + react-single
pnpm mockserver              # :4010 OIDC stub + /api fixtures
pnpm arq:fe:angular-single:mock
pnpm arq:fe:react-single:mock
# Runbook: docs/runbooks/mockserver.md

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
| `pnpm install` 404 en `@base/ionic-*` / `@base/react-native-*` | Dep tipada como `"0.0.0"` en vez de `workspace:*` | En `package.json` de la lib: `"@base/…": "workspace:*"` (nunca versión fija de paquete privado). Luego `pnpm install`. |
| `check:workspace-deps:strict` falla | Import `@base|@josanz|@arquetipos|@saas/*` sin declarar en el consumidor | `pnpm add <pkg> --filter <consumer> --workspace` — ver [workspace-packages.md](../frontend/workspace-packages.md); plan [F52-A2](../plans/rounds/plans-52-fifty-two-round/1750000071000-f52-fix-undeclared-workspace-deps.md). |
| Language server Nx no responde / cuelga | Node fuera de rango para Nx | 1) Recargar VS Code. 2) `pnpm nx reset`. 3) `pnpm check:node-nx` para verificar compatibilidad. |
| Mismatch tipos React 18/19 en React Native | `@types/react` 19.x raíz vs 18.x RN | Pin Metro vía `tools/metro/create-arquetipos-expo-metro-config.cjs` (`disableHierarchicalLookup` + `extraNodeModules`) — [add-mobile-domain.md](./add-mobile-domain.md). Casts locales solo si queda mismatch de types. |
| Pantalla blanca RN web + error `$$typeof` en ArqInput | React 19 + React 18 en el mismo bundle | Reiniciar Expo con `--clear` tras arreglar Metro (`tools/metro/…`) |

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

## Compatibilidad Node ↔ Nx

El workspace usa Nx `23.x`, que soporta Node `20.x` y `22.x`. No usar Node `24.x` con esta versión de Nx.

Para verificar compatibilidad:

```bash
pnpm check:node-nx
```

Si el script falla, actualiza Node a una versión compatible (ver matriz en `docs/plans/global/F00-node-nx-compatibility.md`).
