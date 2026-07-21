# F56-A1 — Build smoke apps arquetipos (compilan)

## Estado

listo para ejecutar

## Objetivo

Probar de forma repetible que las plantillas FE arquetipos **compilan**
(`nx build`), no solo typecheck de libs. Canario mínimo: `angular-single` +
`react-single`. Opcional multi / next / ionic según presupuesto CI.

## Prerrequisitos

- F55 cerrada; Storybook/native-ui estables.
- No exigir mockserver aún (A1 = bundler only).

## Tareas

1. Inventariar targets `build` de apps bajo `apps/arquetipos/frontend/**`.
2. Script o Nx target agregado, p. ej. `pnpm arq:fe:build:smoke`:
   ```bash
   pnpm nx run-many -t build -p angular-single,react-single --parallel=2
   ```
3. CI: job `frontend` o nightly — soft primero (`continue-on-error`) si el
   tiempo supera budget; promover a fail cuando sea estable.
4. Documentar en [local-development.md](../../../guides/local-development.md)
   y [parity.md](../../../arquetipos/parity.md).
5. Si falla build: fix (exports/paths, budgets Angular, Vite aliases) — no
   bajar quality gates.

## Criterios de aceptación

- [ ] `angular-single` + `react-single` build verdes localmente.
- [ ] Comando documentado + referencia en CI (soft o strict).
- [ ] Fallos tipados (raw TS / not found) resueltos o issue abierto.
