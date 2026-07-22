# Isomorphic validation — F65-B1 close / F67-B2 carry

## Decision (F65)

| Item | Status |
|------|--------|
| ADR | [ADR 0012](../adr/adr-0012-isomorphic-validation.md) — **class-validator** SoT for Nest; pure predicates in `@base/shared` for FE |
| Pilot | `validateCreateClient` / `validateUpdateClient` (+ `is*Valid`) in `@base/shared` |
| Zod / dual-schema kit | **Target [F67-B2](../plans/rounds/plans-67-sixty-seven-round/1750000244000-f67-carry-zod-kit.md)** — capacity + BE↔shared DTO drift (`clients.dtos.ts` Nest-only); predicates + ADR 0012 remain SoT |
| Owner | platform shared / FE+BE |
| Blocker for Zod | Capacity + Nest-only DTO drift; no ADR 0013 until piloto Zod ↔ predicates demonstrated |

Do **not** leave as silent “listo”. Resume Zod kit via F67-B2 (or defer F68)
with owner + ADR addendum; predicates stay FE runtime SoT until paridad.
