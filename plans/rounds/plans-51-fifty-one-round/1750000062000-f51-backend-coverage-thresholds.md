# F51-A3 — Cobertura backend audit/settings/tenants + gate

## Estado

completado

## Objetivo

F50 añadió ≥ 10 tests pero no midió cobertura formal. Instrumentar y fijar
umbral ≥ 80% en el scope audit / settings / tenants (o justificar gaps).

## Resultado

**Config** (`libs/base/backend/jest.config.cts`):

- `collectCoverageFrom` acotado a `domains/{audit,settings,tenants}/**`.
- Exclusiones: `audit-hash.ts`, `audit.extension.ts`, `audit.entity.ts`,
  Prisma repos (`*.prisma.repository.ts`), barrels/modules/DTOs/commands/queries/ports.
- Umbrales: global + por dominio — statements/functions/lines **80%**, branches **65%**.
- Output: `coverage/libs/base/backend`.

**Scripts root:**

| Script | Uso |
|--------|-----|
| `pnpm test:cov` | Suite + coverage |
| `pnpm test:cov:check` | Coverage + summary (gate local) |
| `pnpm test:cov:domains` | Solo specs audit/settings/tenants + coverage |

**Specs añadidos:** `audit.mappers.spec.ts`, `tenant.errors.spec.ts`,
`settings.service.spec.ts`.

**Medición local** (`testPathPatterns=domains/(audit|settings|tenants)/`):

| Métrica | Valor |
|---------|-------|
| Statements | ~96% |
| Branches | ~78% |
| Functions | ~94% |
| Lines | ~95% |

Doc: [testing-pyramid.md](../../../guides/testing-pyramid.md) § cobertura F51-A3.
CI soft warn → F52 si se estabiliza.

## Criterios de aceptación

- [x] Reporte coverage reproducible localmente.
- [x] audit / settings / tenants ≥ 80% statements (exclusiones documentadas).
- [x] Doc de cómo leer el reporte en biblia/guides.
