# Isomorphic validation — F65-B1 close / F69-B2 carry

## Decision (F65)

| Item | Status |
|------|--------|
| ADR | [ADR 0012](../adr/adr-0012-isomorphic-validation.md) — **class-validator** SoT for Nest; pure predicates in `@base/shared` for FE |
| Pilot | `validateCreateClient` / `validateUpdateClient` (+ `is*Valid`) en `@base/shared` |
| Zod / dual-schema kit | **Defer F70** — capacidad + BE↔shared DTO drift (`clients.dtos.ts` Nest-only); predicates + ADR 0012 remain SoT |
| Owner | platform shared / FE+BE |
| Blocker for Zod | Capacidad + Nest-only DTO drift; sin ADR 0013 hasta piloto Zod ↔ predicates demostrado |
| F69-B2 close | Defer F70 con owner + blocker explícito (sin silencios) |

Do **not** leave as silent “listo”. Resume Zod kit via **F70** 
with owner + ADR addendum; predicates stay FE runtime SoT until paridad.

Carry record: [F68-B2](../plans/rounds/plans-68-sixty-eight-round/1750000254000-f68-carry-zod-kit.md)
(defer F69 → F70).
