<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Database migrations runbook (F8-DX6 / F38-H6)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Schema artifacts vs Nest runtime

Prisma aparece en **dos lugares** de `@base/backend`; no son duplicados del mismo concepto:

| Ubicación | Qué es | Cuándo tocarlo |
|-----------|--------|----------------|
| `libs/base/backend/prisma/{single,multi}/` | Schema, historial de migraciones, `prisma generate` → `@base/prisma-single` / `@base/prisma-multi` | `migrate dev` / `migrate deploy`, cambios de modelo |
| `libs/base/backend/src/lib/platform/kernel/prisma/` | `PrismaModule`, servicios DI, `PRISMA_SERVICE`, resolución de `DATABASE_URL` | Wiring Nest, extensiones de client, tests de bootstrap |

El **deploy de migraciones** opera sobre `prisma/*/migrations`. El **arranque de la app** importa `PrismaModule` del kernel; el servicio activo delega al client generado del schema que corresponda a `TENANT_MODE`.

Detalle de decisión single/multi schema: [adr-0002](../adr/adr-0002-prisma-multi-single-tenancy.md).

## Matrix

| App / product | Prisma schema | Database (local docker 5440) | Env var (priority) | Deploy command |
|---------------|---------------|------------------------------|--------------------|----------------|
| Josanz / single-tenant | `libs/base/backend/prisma/single/schema.prisma` | `arquetipos` or dedicated `josanz` | `JOSANZ_DATABASE_URL` → `DATABASE_URL` | `pnpm migrate:deploy:single` |
| Arquetipos templates / multi | `libs/base/backend/prisma/multi/schema.prisma` | `arquetipos_multi` (separate DB) | `DATABASE_URL` | `DATABASE_URL=…/arquetipos_multi pnpm migrate:deploy` |
| Arquetipos single template | `single/schema.prisma` | `arquetipos` or dedicated | `ARQUETIPOS_DATABASE_URL` → `DATABASE_URL` | `pnpm migrate:deploy:single` |
| `clients-ms` (microservicio) | `multi/schema.prisma` (mismo kernel) | compartida con monolith o `arquetipos_clients` | `CLIENTS_MS_DATABASE_URL` → `DATABASE_URL` | `DATABASE_URL=… pnpm migrate:deploy` |
| Verifactu CRM SaaS | `apps/productos-saas/verifactu-crm-api/prisma/schema.prisma` | `generic_crm` | `VERIFACTU_DATABASE_URL` → `DATABASE_URL` | `DATABASE_URL=…/generic_crm pnpm saas:crm:migrate` |

**Important:** single and multi schemas must **not** share one database — table shapes differ (`tenantId`).

## Multi vs single at runtime

| `TENANT_MODE` | Prisma service | Schema folder |
|---------------|----------------|---------------|
| `multi` (default for templates) | `PrismaMultiService` | `prisma/multi` |
| `single` | `PrismaSingleService` | `prisma/single` |

`PrismaModule` picks one via `isMultiTenant()` and exposes it as `PRISMA_SERVICE`.
URL resolution: `resolveDatabaseUrl` / `applyProductDatabaseUrl` /
`resolveDatabaseUrlOrDefault` in
`libs/base/backend/src/lib/platform/kernel/prisma/resolve-database-url.ts`.
Provider detection + adapters: `resolveDatabaseProvider` /
`resolveAndCreateAdapter` (see [database-providers.md](./database-providers.md),
[ADR 0014](../adr/adr-0014-database-provider-portability.md)).

Supported URL schemes: `postgresql://` / `postgres://` (primary),
`sqlserver://` / `mssql://`, `mysql://` / `mariadb://`. Optional override:
`DATABASE_PROVIDER`. Shadow schemas for validate:
`schema.mssql.prisma` / `schema.mysql.prisma` (regenerate with
`pnpm prisma:generate-provider-schemas`).

## Migration timestamps & parallel PRs

Prisma migration folders are timestamp-prefixed (`YYYYMMDDHHMMSS_name`).

1. **Never** invent a timestamp by hand that collides with an existing folder.
2. Prefer `pnpm prisma migrate dev` (or the package scripts) so Prisma allocates
   the clock.
3. If two PRs land migrations with the **same** timestamp against the same
   history → `migrate deploy` fails. Rebase the later PR and regenerate the
   migration folder name.
4. Products that **extend** `@base/backend` schemas (Josanz, SaaS) keep their
   **own** migration history directory — do not copy base migration SQL into
   product folders without a coordinated rename.

## CI: one writer per migration history

| History path | Allowed concurrent writer |
|--------------|---------------------------|
| `libs/base/backend/prisma/multi/migrations` | **One** CI job / deploy step |
| `libs/base/backend/prisma/single/migrations` | **One** CI job / deploy step |
| Product schemas (CRM, etc.) | **One** job per database |

Do **not** run two `migrate deploy` processes against the same database in
parallel (matrix shards, two apps sharing a DB). Serialize with a job
`needs:` / mutex, or give each app its own database (preferred for multi vs
single).

`migrate deploy` is additive; this lote does **not** author destructive
migrations (`DROP` without expand/contract).

## Local bootstrap

```powershell
# Josanz (default .env TENANT_MODE=single)
$env:DATABASE_URL="postgresql://arquetipos:arquetipos@localhost:5440/arquetipos"
pnpm dev:infra
pnpm josanz:bootstrap
```

`josanz:bootstrap` runs migrate + prisma generate + demo seed (8 clients, 12 eventos vía `Project`).

Re-seed only (idempotent):

```bash
pnpm josanz:seed-demo
```

Multi-tenant templates:

```powershell
$env:DATABASE_URL="postgresql://arquetipos:arquetipos@localhost:5440/arquetipos_multi"
pnpm migrate:deploy

# CRM
$env:DATABASE_URL="postgresql://arquetipos:arquetipos@localhost:5440/generic_crm?schema=public"
pnpm saas:crm:migrate
pnpm saas:crm:generate
```

Create extra DBs once:

```sql
CREATE DATABASE arquetipos_multi OWNER arquetipos;
CREATE DATABASE generic_crm OWNER arquetipos;
-- Optional dedicated Josanz DB:
CREATE DATABASE josanz OWNER arquetipos;
```

## Per-app database URL (ports / adapters)

Each app bootstrap calls `applyProductDatabaseUrl()` from `@base/backend`, mapping a product-specific env var onto `DATABASE_URL` before Prisma loads:

| App | Bootstrap | Keys (first wins) |
|-----|-----------|-------------------|
| `josanz-api` | `apps/clientes/josanz/backend/src/bootstrap-env.ts` | `JOSANZ_DATABASE_URL`, `DATABASE_URL` |
| `ideauto-recalls-api` | *(M0 stub — wire bootstrap in M1+ / F83 MSSQL)* | TBD |
| `api-single` | `apps/arquetipos/backend/monolith/api-single/src/bootstrap-env.ts` | `ARQUETIPOS_DATABASE_URL`, `DATABASE_URL` |
| `clients-ms` | `apps/arquetipos/backend/microservices/clients-ms/src/bootstrap-env.ts` | `CLIENTS_MS_DATABASE_URL`, `DATABASE_URL` |
| `verifactu-crm-api` | `resolveCrmDatabaseUrl()` | `VERIFACTU_DATABASE_URL`, `DATABASE_URL` |

Domain code depends on **repository ports**; infrastructure binds Prisma adapters in the Nest module (pilot: `JosanzFleetModule` → `FLEET_VEHICLE_REPOSITORY` / `PrismaFleetVehicleRepository`).

## CI guard

Migration SQL must be UTF-8 (no embedded nulls):

```bash
node tools/migrate/fix-migration-encoding.mjs --check
```

## Parity

Single/multi schemas must stay aligned except `tenantId`:

```bash
pnpm check:schema-parity
```

## Specs

```bash
pnpm exec jest -c libs/base/backend/jest.config.ts --rootDir libs/base/backend \
  src/lib/platform/kernel/prisma/resolve-database-url.spec.ts \
  src/lib/platform/kernel/prisma/prisma-services.spec.ts
```
