# Database provider compatibility

Maps Prisma features used in `@base/backend` schemas across engines.

| Feature (Postgres SoT) | PostgreSQL | MySQL / MariaDB (shadow) | SQL Server (shadow) |
|------------------------|------------|--------------------------|---------------------|
| `String[]` | native array | `Json` | `String` (JSON text) |
| `Json` / `Json?` | yes | yes | `String` / `String?` |
| `@default(cuid())` | yes | yes | yes |
| `@db.Date` | yes | yes | yes |
| `onDelete: Cascade` multi-path | ok | ok | remapped to `NoAction` (SQL Server restriction) |
| Native arrays in queries | yes | app-level JSON | app-level JSON string |

## Generation

```bash
node tools/prisma/generate-provider-schemas.mjs
```

Do **not** hand-edit `schema.mssql.prisma` / `schema.mysql.prisma`.

## Runtime

Connection adapters: `resolveDatabaseProvider` + `createAdapterForProvider` in
`libs/base/backend/src/lib/platform/kernel/prisma/`.

Policy: [ADR 0014](../adr/adr-0014-database-provider-portability.md).
Runbook: [database-providers.md](../runbooks/database-providers.md).
