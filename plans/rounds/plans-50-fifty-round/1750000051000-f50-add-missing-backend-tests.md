# F50-A2 — Añadir tests faltantes en dominios backend

## Estado

completado

## Origen

Carry de [F49-A4](../plans-49-forty-nine-round/1750000043000-f49-add-missing-backend-tests.md).

## Resultado

Nuevos / ampliados (≥ 10 casos):

- `audit.service.spec.ts` — SYSTEM actor + tenant null; actor de request; seed vacío
- `audit-interceptor.spec.ts` — user/sub/tenant/ip + request missing
- `audit.handlers.spec.ts` — empty seed
- Settings: paginación `ListSettings` + `ConflictException` key duplicada
- Tenants: slug duplicado + status `Suspended` / `Pending`
- Billing / Inventory / Projects: edges `findPage` vacío

Verificación: `pnpm nx test base-backend` → **111 suites / 321 tests** verdes.

Cobertura formal ≥ 80% por dominio no instrumentada en CI en esta ronda;
umbral + reporte → **F51-A3**.

## Criterios de aceptación

- [x] ≥ 10 tests nuevos.
- [x] Suite `base-backend` verde (justificación cobertura → F51).
- [x] `pnpm nx test base-backend` verde.
