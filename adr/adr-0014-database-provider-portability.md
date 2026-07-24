# ADR 0014 — Database provider portability

- Status: **accepted**
- Date: 2026-07-24
- Tags: prisma, database, portability, mssql, mysql

## Context

The monorepo historically assumed PostgreSQL end-to-end (`PrismaPg`,
`provider = "postgresql"`). Cliente Ideauto (Recalls) and other deployments need
**SQL Server** (and optionally MySQL/MariaDB) without rewriting domain modules.

## Decision

1. **Postgres is primary** for local docker, CI default, and migration history under
   `libs/base/backend/prisma/{single,multi}/schema.prisma`.
2. **Provider detection** is centralized in
   `resolveDatabaseProvider` (URL scheme + optional `DATABASE_PROVIDER` override).
   Unknown schemes fail fast — no silent fallback to Postgres.
3. **Driver adapters** are created only in the kernel factory
   (`createAdapterForProvider` / `resolveAndCreateAdapter`). Domain
   `*-prisma.repository` code must not import `@prisma/adapter-*`.
4. **Shadow schemas** `schema.mssql.prisma` / `schema.mysql.prisma` are generated
   from the Postgres SoT for `prisma validate` and document type remaps
   (`String[]` / `Json` differences). They are **not** the generate target for
   `@base/prisma-single|multi` until a product explicitly opts in.
5. **Best-effort** secondary engines: connection + validate in F83; real
   `migrate deploy` / seeds / CI matrix are follow-up work (F85).

## Consequences

- Changing `DATABASE_URL` scheme (or `DATABASE_PROVIDER`) switches the Nest Prisma
  adapter without touching domain handlers.
- Feature parity across engines is **not** guaranteed (arrays, JSON, cascades).
  See [database-provider-compat.md](../backend/database-provider-compat.md).
- SaaS entrypoints that still construct `PrismaPg` directly should migrate to the
  kernel factory (tracked in F85).

## Links

- [database-providers.md](../runbooks/database-providers.md)
- [database-migrations.md](../runbooks/database-migrations.md)
- [ADR 0002](./adr-0002-prisma-multi-single-tenancy.md)
