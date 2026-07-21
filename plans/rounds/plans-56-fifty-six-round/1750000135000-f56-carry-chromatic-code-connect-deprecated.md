# F56-D1 — Carry F55: Chromatic / Code Connect / remove deprecated

## Estado

listo para ejecutar

## Objetivo

Resolver o re-diferir con evidencia el carry de F55:

1. **Chromatic / visual SB** (F55-B1) — token o Playwright screenshots SB.
2. **Figma Code Connect** piloto button/input/alert (F55-D1).
3. **Remove** primitivos `@deprecated` framework-only (F54-A3) cuando
   `check:deprecated` / native-first lo permitan.

## Tareas

1. Chromatic: si hay `CHROMATIC_PROJECT_TOKEN` → soft nightly; si no → defer F57
   y mantener `build-storybook` + opción PW screenshots de static SB.
2. Code Connect: si hay acceso Figma MCP → piloto; si no → defer F57 (matriz ya
   actualizada).
3. Audit `pnpm check:deprecated` / imports legacy UI; PR de remove acotado o
   lista explícita → F57.
4. No bloquear A1/C1/C2 por falta de tokens.

## Criterios de aceptación

- [ ] Cada ítem: hecho **o** defer F57 con motivo en Resultado.
- [ ] Sin regresiones ownership UI / native-first.
