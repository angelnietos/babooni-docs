# Isomorphic validation — F65-B1 close / F66-B2 carry

## Decision (F65)

| Item | Status |
|------|--------|
| ADR | [ADR 0012](../adr/adr-0012-isomorphic-validation.md) — **class-validator** SoT for Nest; pure predicates in `@base/shared` for FE |
| Pilot | `validateCreateClient` / `validateUpdateClient` (+ `is*Valid`) in `@base/shared` |
| Zod / dual-schema kit | **Carry F66-B2** — no second library without superseding ADR / addendum |
| Owner | platform shared / FE+BE |
| Blocker for Zod | Capacity + BE↔shared DTO drift (`clients.dtos.ts` Nest-only) |

Do **not** leave as silent “listo”. Resume Zod kit only via
[F66-B2](../plans/rounds/plans-66-sixty-six-round/1750000233000-f66-carry-zod-kit.md)
(or defer F67 with owner).
