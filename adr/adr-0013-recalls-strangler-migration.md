# Recalls migration — strangler into client Ideauto (not SaaS)

- Status: proposed
- Date: 2026-07-24
- Deciders: platform + producto Ideauto Recalls

## Context

Recalls_v2 (`ideauto-server` + `ideauto-client`) is Ideauto's production product for vehicle recall campaigns (DGT SOAP, waves, VIN files, budgets/invoices). It runs as two standalone pnpm workspaces without Nx layering or `@base/*` reuse.

The Arquetipos monorepo places **customer products** under `apps/clientes/{slug}` + `libs/clientes/{slug}` with npm scope `@slug/*` and tag `layer:clientes` (same pattern as Josanz). **SaaS** products (`apps/productos-saas`, `@saas/*`) are a different layer (e.g. Verifactu) and must not host Ideauto Recalls.

Database access to the existing MSSQL estate depends on provider portability (round F83).

## Decision

1. **Placement:** rebuild Recalls as client product **Ideauto**:
   - `apps/clientes/ideauto/recalls/{backend,frontend}`
   - `libs/clientes/ideauto/` → `@ideauto/*`
   - Tags: `layer:clientes`
   - May import `@base/*` only (not `@arquetipos/*`, `@saas/*`, `@josanz/*`)

2. **Migration style:** **strangler fig** (vertical slices: auth → campaigns/waves → budgets/invoices → DGT → reports/admin → cutover). Keep legacy behind proxy/feature flags; share MSSQL during transition. Reject big-bang as default while DGT/PDF parity remain open.

## Consequences

### Positive

- Aligns with client-product conventions ([nuevo-cliente-checklist](../clientes/nuevo-cliente-checklist.md)).
- Clear brand/boundary separation from Verifactu SaaS and Josanz ERP.
- Rollback via routing flags; auth can move first; DGT parallel-run possible.

### Negative / accepted costs

- Temporary dual maintenance (hotfixes only on legacy).
- Shared-DB discipline during strangler.

### Follow-ups

- Execute M0–M6: [runbooks/recalls-migration.md](../runbooks/recalls-migration.md).
- Docs: [architecture/recalls-*.md](../architecture/recalls-v2-assessment.md).
- Round: [plans-84](../plans/rounds/plans-84-eighty-four-round/).

## References

- [recalls-v2-assessment.md](../architecture/recalls-v2-assessment.md)
- [recalls-domain-mapping.md](../architecture/recalls-domain-mapping.md)
- [adr-0005-jwt-vs-keycloak.md](./adr-0005-jwt-vs-keycloak.md)
- [adr-0008-platform-scope-vs-mvp-client.md](./adr-0008-platform-scope-vs-mvp-client.md)
- [plans-83](../plans/rounds/plans-83-eighty-three-round/)
