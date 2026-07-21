# F55-D2 — Storybook targets arquetipos UI (configs huérfanos)

## Estado

listo para ejecutar

## Objetivo

Carry F53-D2: existen `.storybook/` en `arquetipos-angular-ui` /
`arquetipos-react-ui` sin targets Nx `storybook` / `build-storybook` fiables.
F55 los cablea o depreca explícitamente.

## Tareas

1. Inventariar configs orphan vs targets actuales.
2. Añadir `storybook` + `build-storybook` (patrón react-ui / angular-ui) **o**
   documentar “solo base-native-ui + base-*-ui; arquetipos SB deprecated”.
3. Owner tags en catálogo si se mantienen.
4. No exigir Chromatic de marca arquetipos en esta ronda.

## Criterios de aceptación

- [ ] Targets verdes **o** deprecación documentada en design-system.
- [ ] Sin configs muertas sin nota.
