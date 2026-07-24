# Recalls migration — strangler fig over big-bang

- Status: proposed
- Date: 2026-07-24
- Deciders: platform + producto Recalls (Ideauto)

## Context

Recalls_v2 (`ideauto-server` + `ideauto-client`) is a production product for vehicle recall campaigns (DGT SOAP, postal/telematic waves, VIN files, budgets/invoices). It runs as two standalone pnpm workspaces: Express/Sequelize/MSSQL backend and Next/React frontend, without Nx layering or `@base/*` reuse.

The Arquetipos monorepo is the target home for a rebuilt Recalls SaaS product (`apps/productos-saas/recalls`, `@saas/*` on top of `@base/*`). Database access to the existing MSSQL estate depends on provider portability work (round F83).

We must choose a migration style that preserves business continuity (especially DGT) while stopping further investment in the legacy architecture.

## Decision

Adopt a **strangler-fig migration**: migrate vertical slices (auth → campaigns/waves → budgets/invoices → DGT → reports/admin → cutover), keep legacy alive behind a reverse proxy / feature flags, share MSSQL during transition, and only decommission Express/Next legacy after parity gates pass.

Reject a **big-bang cutover** as the default plan while external DGT contracts, document/PDF parity, and on-disk campaign file layouts remain.

## Consequences

### Positive

- Rollback is a routing flag, not a weekend restore.
- Security-critical auth can move first without waiting for full feature parity.
- DGT can run in parallel (legacy + new adapter) before switching.
- Platform investments (Prisma adapters, Nest hex, FE 4-layer contract, CI gates) land incrementally.

### Negative / accepted costs

- Temporary dual maintenance (hotfixes only on legacy).
- Proxy/flag operational complexity.
- Shared-DB discipline required (no destructive migrations without dual-write or freeze).

### Follow-ups

- Execute milestones M0–M6 per [runbooks/recalls-migration.md](../runbooks/recalls-migration.md).
- Keep product docs in [architecture/recalls-*.md](../architecture/recalls-v2-assessment.md).
- Round tracking: [plans-84](../plans/rounds/plans-84-eighty-four-round/).

## References

- [recalls-v2-assessment.md](../architecture/recalls-v2-assessment.md)
- [recalls-migration-strategy.md](../architecture/recalls-migration-strategy.md)
- [adr-0005-jwt-vs-keycloak.md](./adr-0005-jwt-vs-keycloak.md)
- [adr-0008-platform-scope-vs-mvp-client.md](./adr-0008-platform-scope-vs-mvp-client.md)
- [plans-83](../plans/rounds/plans-83-eighty-three-round/)
