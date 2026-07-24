# Database providers runbook (F83)

Postgres remains the **primary** local/CI engine. SQL Server and MySQL/MariaDB are
supported at the **connection + schema-validate** layer (best-effort). Full migrate
deploy / seeds per engine → **F85**.

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

## Switching provider (runtime)

Kernel services (`PrismaSingleService` / `PrismaMultiService`) call
`resolveAndCreateAdapter(url)` which picks:

- `@prisma/adapter-pg`
- `@prisma/adapter-mssql`
- `@prisma/adapter-mariadb`

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
```

## Troubleshooting

| Symptom | Check |
|---------|-------|
| `Unsupported DATABASE_URL scheme` | Fix scheme or set `DATABASE_PROVIDER` |
| Adapter package missing | `pnpm add -w @prisma/adapter-mssql` / `@prisma/adapter-mariadb` |
| SQL Server collation / Unicode | Prefer `Latin1_General_CI_AS` or UTF-8 collations; store JSON-as-string fields as `nvarchar` |
| MySQL charset | Use `utf8mb4` |
| Multi-tenant cascades on MSSQL | Shadow schema uses `NoAction`; app must delete in order |

## Related

- [database-provider-compat.md](../backend/database-provider-compat.md)
- [ADR 0014](../adr/adr-0014-database-provider-portability.md)
- [ADR 0002](../adr/adr-0002-prisma-multi-single-tenancy.md) — single vs multi tenancy
