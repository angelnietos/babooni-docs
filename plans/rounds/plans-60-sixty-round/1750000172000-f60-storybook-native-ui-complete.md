# F60-B1 — Storybook native-ui completo

## Estado

listo para ejecutar

## Objetivo

Cumplir ADR 0011: el catálogo primario es **`base-native-ui`**. Hoy casi solo
existe un catálogo genérico
([`catalog.stories.tsx`](../../../../libs/base/frontend/crosscutting/native-ui/src/lib/catalog.stories.tsx)).
F60 añade **stories por átomo** con estados reales.

## Entregables

1. Story por átomo canario (A2): Default, Disabled, Loading (si aplica), Error /
   Danger, Sizes, Dark/atmosphere si el brief lo define.
2. Targets Nx `storybook` + `build-storybook` verdes (puerto doc ~4400).
3. Docs MDX o descripción: cuándo usar wrapper `Native*` vs CE directo.
4. CI: al menos `build-storybook` soft o nightly (enlace F60-E2 Chromatic).

## Criterios de aceptación

- [ ] `pnpm nx run base-native-ui:storybook` arranca y muestra átomos.
- [ ] `pnpm nx run base-native-ui:build-storybook` sin error.
- [ ] Ownership: stories en el paquete que posee el CE (ADR 0011).

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
```

## Depende de

F60-A2 (idealmente; puede empezar con stories stub y rellenar tras polish).

## Relacionado

F60-B2 (adapters), F60-E2 (Chromatic).
