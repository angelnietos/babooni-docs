# F56-D1 — Carry F55: Chromatic / Code Connect / remove deprecated

## Estado

defer F57

## Objetivo

Resolver o re-diferir Chromatic, Code Connect y remove `@deprecated`.

## Criterios de aceptación

- [x] Cada ítem: hecho **o** defer F57 con motivo.
- [x] Sin regresiones ownership UI.

## Resultado

**Defer F57:** sin `CHROMATIC_PROJECT_TOKEN` ni acceso Figma Code Connect en CI.
Remove de primitivos `@deprecated` no ejecutado en esta ronda.
`build-storybook` base UI sigue como señal de rotura de stories.
