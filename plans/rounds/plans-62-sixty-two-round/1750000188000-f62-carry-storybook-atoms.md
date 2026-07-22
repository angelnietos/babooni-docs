# F62-D3 — Storybook SoT + adapters brand (carry F60-B1/B2)

## Estado

listo para ejecutar

## Objetivo

Cerrar Storybook deferred de F60:

| Origen | Título |
|--------|--------|
| [F60-B1](../plans-60-sixty-round/1750000172000-f60-storybook-native-ui-complete.md) | Stories por átomo en `base-native-ui` |
| [F60-B2](../plans-60-sixty-round/1750000173000-f60-storybook-adapters-brand.md) | Adapters Angular/React / brand |

ADR 0011: catálogo primario = Lit; adapters **solo** si cambian API visual.

## Entregables

### SoT (`base-native-ui`)

1. Story por átomo canario (button, input, select, alert, card, badge, spinner,
   empty-state, …) con estados reales (hover/focus/disabled/error/loading).
2. `pnpm nx run base-native-ui:storybook` usable; `build-storybook` verde.
3. Enlace desde design-system: “abrir native-ui primero”.

### Adapters / brand

1. Inventario: wrappers `Native*` Angular/React y `@arquetipos/*-ui` con delta
   visual vs CE.
2. Stories mínimas solo donde hay delta (props/events, host class brand).
3. Ionic/Next UI: documentar si exponen wrapper propio; sin duplicar catálogo.
4. Orphan Storybook configs sin target Nx → limpiar o cablear.

## Criterios de aceptación

- [ ] `build-storybook` `base-native-ui` verde.
- [ ] Select + button + input con estados documentados (post A2/A3).
- [ ] `base-angular-ui` / `base-react-ui` `build-storybook` documentados o
      skip justificado (sin delta).
- [ ] Sin re-story completo de átomos Lit en brand packages.

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
pnpm nx run base-angular-ui:build-storybook
pnpm nx run base-react-ui:build-storybook
```

## Depende de

F62-A2, F62-A3.

## Bloquea

F62-D2 (Chromatic).
