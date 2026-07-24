# Database providers runbook

Postgres remains the **primary** local/CI engine. SQL Server and MySQL/MariaDB are
supported for connection + schema-validate + optional `db push` smoke. Provider-aware
seeds and Azure SQL hardening continue under **F86-G**.

## Env

| Variable | Role |
|----------|------|
| `DATABASE_URL` | Connection string (scheme selects provider unless overridden) |
| `DATABASE_PROVIDER` | Optional override: `postgresql` \| `sqlserver` \| `mysql` \| `sqlite` |

### URL schemes

| Provider | Examples |
|----------|----------|
| PostgreSQL | `postgresql://user:pass@localhost:5432/db`, `postgres://…` |
| SQL Server | `sqlserver://…`, `mssql://…`, `jdbc:sqlserver://…` |
| MySQL / MariaDB | `mysql://…`, `mariadb://…` |
| SQLite | `file:./dev.db` (detection only; Nest wiring not shipped) |

## Local Postgres (default)

See [database-migrations.md](./database-migrations.md) and `docker-compose.dev.yml`.

```bash
pnpm migrate:deploy
pnpm prisma:generate
```

## Secondary engines (local smoke)

```bash
docker compose -f docker-compose.providers.yml up -d
```

| Engine | Script (validate only) | Live smoke (`db push`) |
|--------|------------------------|-------------------------|
| SQL Server | `pnpm migrate:deploy:mssql` | set `DATABASE_URL` + `pnpm migrate:smoke:mssql` |
| MySQL | `pnpm migrate:deploy:mysql` | set `DATABASE_URL` + `pnpm migrate:smoke:mysql` |

Example SQL Server:

```bash
# PowerShell
$env:DATABASE_URL='sqlserver://sa:Your_password123@localhost:1433;database=arquetipos;encrypt=true;trustServerCertificate=true'
$env:DATABASE_PROVIDER='sqlserver'
pnpm migrate:smoke:mssql
```

Example MySQL (compose maps host `3307`):

```bash
$env:DATABASE_URL='mysql://arquetipos:arquetipos@localhost:3307/arquetipos'
$env:DATABASE_PROVIDER='mysql'
pnpm migrate:smoke:mysql
```

Seeds for secondary engines: use Postgres seeds locally; `pnpm seed:mssql` /
`seed:mysql` are explicit stubs until F86-G lands provider-aware seed history
(expand/contract). Do not treat those scripts as “broken product features”.

CI: [`.github/workflows/db-providers-smoke.yml`](../../.github/workflows/db-providers-smoke.yml)
validates shadow schemas on PRs that touch Prisma; live push is opt-in via repo vars
`ENABLE_MSSQL_SMOKE` / `ENABLE_MYSQL_SMOKE` (does **not** block main).

## Switching provider (runtime)

Kernel services (`PrismaSingleService` / `PrismaMultiService`) and SaaS entrypoints
call `resolveAndCreateAdapter(url)` from `@base/backend/prisma` which picks:

- `@prisma/adapter-pg`
- `@prisma/adapter-mssql`
- `@prisma/adapter-mariadb`

**Do not** construct `PrismaPg` / `PrismaMssql` / `PrismaMariaDb` outside
`libs/base/backend/.../prisma/adapters.ts`. Gate: `pnpm check:db-portability`.

```bash
# Example — SQL Server (requires reachable server + matching schema deploy)
set DATABASE_URL=sqlserver://sa:Your_password123@localhost:1433;database=arquetipos
set DATABASE_PROVIDER=sqlserver
```

## Validate shadow schemas

```bash
node tools/prisma/generate-provider-schemas.mjs
pnpm check:db-migrations
pnpm check:db-portability
pnpm migrate:deploy:mssql
pnpm migrate:deploy:mysql
```

## Test harness

Jest suites can inject provider + URL via `@base/backend`:

```ts
import { withDatabaseProvider, runWithDatabaseProvider } from '@base/backend';

describe('sqlserver', () => {
  withDatabaseProvider({
    provider: 'sqlserver',
    url: 'sqlserver://localhost/db',
  });
  // …
});
```

## Troubleshooting

| Symptom | Check |
|---------|-------|
| `Unsupported DATABASE_URL scheme` | Fix scheme or set `DATABASE_PROVIDER` |
| Adapter package missing | `pnpm add -w @prisma/adapter-mssql` / `@prisma/adapter-mariadb` |
| SQL Server collation / Unicode | Prefer `Latin1_General_CI_AS` or UTF-8 collations; store JSON-as-string fields as `nvarchar` |
| MySQL charset | Use `utf8mb4` |
| Multi-tenant cascades on MSSQL | Shadow schema uses `NoAction`; app must delete in order |
| `db push` fails on secondary | Confirm compose health + URL; Postgres migrate history is not applied as-is |

## Related

- [database-provider-compat.md](../backend/database-provider-compat.md)
- [ADR 0014](../adr/adr-0014-database-provider-portability.md)
- [ADR 0002](../adr/adr-0002-prisma-multi-single-tenancy.md) — single vs multi tenancy
- Active plans: [F86](../plans/rounds/plans-86-eighty-six-round/)
