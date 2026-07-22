<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F46-A2 — Ajustar tsconfig de `base-react-ui` para excluir stories de spec</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Excluir archivos `*.stories.tsx` del typecheck de spec en `base-react-ui`
para eliminar falsos positivos de Storybook (`@storybook/react-vite`) y
errores de tipos en stories.

## Tareas

1. Abrir `libs/base/frontend/react/platform/ui/tsconfig.spec.json`.
2. Agregar `"**/*.stories.tsx"` a la lista `exclude`.
3. Ejecutar `pnpm nx typecheck base-react-ui` para confirmar que pasa.

## Criterios de aceptación

- [ ] `base-react-ui:typecheck` pasa sin errores.
- [ ] Stories siguen excluidas de `tsconfig.lib.json` (ya están).
