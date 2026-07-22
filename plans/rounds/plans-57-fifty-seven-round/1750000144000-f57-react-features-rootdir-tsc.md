<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F57-C1 — Fix TS6059 `arquetipos-react-*-features` (rootDir)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Quitar el workaround `nx build react-single --excludeTaskDependencies` de
`arq:fe:build:smoke` arreglando el fallo `@nx/js:tsc` **TS6059** (file no bajo
`rootDir`) en libs `arquetipos-react-*-features` (y hermanas si aplica).

## Contexto

F56-A1: Angular `angular-single` build OK; React Vite build OK solo si se excluyen
deps de build tsc de features. Causa típica: `rootDir` / `include` / re-exports
fuera del árbol, o paths que meten fuentes de otra lib en la compilación.

## Entregables

1. Reproducir: `pnpm nx run arquetipos-react-clients-features:build` (o el target
   que falle) y capturar TS6059.
2. Fix mínimo: tsconfig (`rootDir`, `include`, `references`) y/o dejar de buildar
   libs no-buildable con `@nx/js:tsc` si deben ser source-only (preferencia monorepo:
   **non-buildable** FE libs consumidas por Vite).
3. Actualizar `arq:fe:build:smoke` para **no** necesitar `--excludeTaskDependencies`
   en React (o documentar por qué el exclude sigue siendo correcto si se elige
   non-buildable y se quita el target build).
4. Nota en [arquetipos/parity.md](../../../arquetipos/parity.md) / how-to-use.

## Criterios de aceptación

- [x] `pnpm arq:fe:build:smoke` verde sin exclude **o** exclude eliminado porque ya no hay target tsc conflictivo.
- [x] Typecheck features React arquetipos sigue verde.
- [x] Sin regresiones Angular single build.

## Verificación

```bash
pnpm arq:fe:build:smoke
pnpm nx typecheck arquetipos-react-clients-features
```
