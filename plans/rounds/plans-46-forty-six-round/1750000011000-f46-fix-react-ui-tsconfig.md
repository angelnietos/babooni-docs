# F46-A2 — Ajustar tsconfig de `base-react-ui` para excluir stories de spec

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
