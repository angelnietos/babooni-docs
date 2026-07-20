# ADR 0002: Prisma multi/single tenancy

- Status: accepted
- Date: 2026-07-08

## Context

Some deployments are multi-tenant (shared DB, `tenantId` column, row-level
scoping); others are single-tenant (one schema, no `tenantId`). We need both
without forking the application code.

## Decision

Maintain two Prisma schemas (`prisma/multi`, `prisma/single`) selected by
`TENANT_MODE` at boot. A single `PrismaModule` resolves `PRISMA_SERVICE` to the
`PrismaMultiService` or `PrismaSingleService` and installs the same
security + audit query extensions on both.

Tenant scoping lives in `TenantContext.scopedWhere`, which is a no-op in
single-tenant mode and ANDs `{ tenantId }` in multi-tenant mode. Repositories
never hardcode the filter, so the same `PrismaRepository` serves both models.

## Consequences

- One application codebase, two deployment shapes.
- The two schemas must stay in parity except for the `tenantId` column (enforced
  by the E.1 parity script/lint). Drift here is the top fragility risk.
- Single-tenant deployments drop the tenant filter automatically — verified by
  the tenant-leak suite (`prisma.repository.spec.ts`).

## See also

- Schema-parity guard (E.1): `tools/scripts/check-schema-parity.mjs` (CI `migrations` job).
- Tenant isolation + soft-delete integration test: `libs/base/backend/src/lib/integration/prisma.repository.int-spec.ts`.
- Auth/token scoping: ADR 0005 (Keycloak) — the principal carries `tenantId`.
- Back to the [docs hub](../README.md).
