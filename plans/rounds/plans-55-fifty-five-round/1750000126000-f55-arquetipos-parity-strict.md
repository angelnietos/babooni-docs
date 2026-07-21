# F55-C2 — `check:arquetipos-parity` strict en CI

## Estado

listo para ejecutar

## Objetivo

F54-B4 dejó el checker en soft. F55 lo promueve a **strict** en el job
frontend (o hygiene) cuando arch/UI/func estén estables.

## Tareas

1. Auditar warns/errors soft actuales.
2. `pnpm check:arquetipos-parity -- --strict` en CI frontend.
3. Actualizar pr-checklist / ci-gates.
4. Si stacks Core/Mobile flaky: mantener `parity_level` en manifest (no Full).

## Criterios de aceptación

- [ ] Strict en CI **o** defer con lista de gaps.
- [ ] Mensaje de fallo apunta a `docs/arquetipos/parity.md`.
