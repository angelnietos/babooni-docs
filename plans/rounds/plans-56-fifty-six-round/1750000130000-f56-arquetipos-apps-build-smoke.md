# F56-A1 — Build smoke apps arquetipos (compilan)

## Estado

completado

## Objetivo

Probar de forma repetible que las plantillas FE arquetipos **compilan**
(`nx build`), no solo typecheck de libs. Canario mínimo: `angular-single` +
`react-single`.

## Criterios de aceptación

- [x] `angular-single` + `react-single` build verdes localmente.
- [x] Comando documentado + referencia en CI (soft o strict).
- [x] Fallos tipados documentados (TS6059 features React → F57).

## Resultado

`pnpm arq:fe:build:smoke` =
`nx build angular-single && nx build react-single --excludeTaskDependencies`.
Angular production OK. React vía Vite sin `^build` de libs tsc (TS6059
rootDir en `arquetipos-react-*-features`). CI soft en job `frontend`.
