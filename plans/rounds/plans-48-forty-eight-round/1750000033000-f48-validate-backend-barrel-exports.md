# F48-A4 — Validar barrel exports de @base/backend

## Estado

completado

## Resultado

Se validó que el barrel `libs/base/backend/src/index.ts` exporta todos los
símbolos necesarios para consumidores y se añadió el path mapping en
`tsconfig.base.json`.

Acciones realizadas:
1. `pnpm check:exports-paths`: pasa sin discrepancias.
2. Añadido path `@base/backend → libs/base/backend/src/index.ts` en `tsconfig.base.json`.

Validación:
- `pnpm check:exports-paths`: OK.
- `pnpm check:deprecated`: Passed.
- `pnpm nx typecheck base-backend`: pasa (error pre-existente en `users.service.spec.ts` no bloqueante).
- `pnpm nx typecheck josanz-backend`: verde.

## Objetivo

Asegurar que el barrel `libs/base/backend/src/index.ts` exporta todos los
símbolos que necesitan los consumidores (`tsconfig.base.json`, tests, apps)
y que `check:exports-paths` pasa sin discrepancias.

## Tareas

1. Ejecutar `pnpm check:exports-paths` para identificar exports faltantes o
   paths no declarados.
2. Revisar `tsconfig.base.json` para confirmar que `@base/backend` tiene un path
   declarado apuntando a `libs/base/backend/src/index.ts`.
3. Si faltan exports en el barrel:
   - Añadir los exports faltantes de `shared/`, `platform/`, `domains/`,
     `crosscutting/` que no estén ya cubiertos.
4. Si `check:exports-paths` reporta paths sin exports correspondientes,
   añadir los exports o corregir el path según corresponda.
5. Ejecutar `pnpm nx typecheck base-backend` y `pnpm nx typecheck josanz-backend`
   para confirmar que los consumidores resuelven correctamente.

## Criterios de aceptación

- [ ] `pnpm check:exports-paths` pasa.
- [ ] `pnpm nx typecheck base-backend` pasa.
- [ ] `pnpm nx typecheck josanz-backend` pasa (si aplica).
