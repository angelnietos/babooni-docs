# Isomorphic validation — F70-B2 close

## Decision (F65)

| Item | Status |
|------|--------|
| ADR | [ADR 0012](../adr/adr-0012-isomorphic-validation.md) — **class-validator** SoT for Nest; pure predicates in `@base/shared` for FE |
| Pilot | `validateCreateClient` / `validateUpdateClient` (+ `is*Valid`) en `@base/shared` |
| Zod / dual-schema kit | **Defer F71** — capacidad + BE↔shared DTO drift (`clients.dtos.ts` Nest-only); predicates + ADR 0012 remain SoT |
| Owner | platform shared / FE+BE |
| Blocker for Zod | Capacidad + Nest-only DTO drift; sin ADR 0013 hasta piloto Zod ↔ predicates demostrado |
| F70-B2 close | Defer F71 con owner + blocker explícito (sin silencios) |

Do **not** leave as silent “listo”. Resume Zod kit via **F71**
with owner + ADR addendum; predicates stay FE runtime SoT until paridad.

Carry record: F68 → F69 → F70 deferred.
