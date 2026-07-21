# F55-D2 — Storybook targets arquetipos UI (configs huérfanos)

## Estado

completado

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

- [x] Targets verdes **o** deprecación documentada en design-system.
- [x] Sin configs muertas sin nota.

## Resultado

Targets `storybook` / `build-storybook` (+ `continuous`) en
`base-native-ui`, `base-angular-ui`, `base-react-ui`, `josanz-angular-ui`,
`arquetipos-angular-ui`, `arquetipos-react-ui`. Plugin `@nx/storybook/plugin` en
`nx.json`. Puertos: 4401–4405 + 6007. Doc: [design-system.md](../../../frontend/design-system.md).
