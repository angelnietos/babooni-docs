<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F53-B2 — Storybook de `@base/native-ui` (serve + build)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

La SoT Lit **no tiene** `.storybook` ni targets. El diseño system debe verse
**sin** arrancar Angular/React. Crear Storybook (Vite + web-components o
addon Lit) con:

- Target **`storybook`** (serve)
- Target **`build-storybook`**
- Stories para los 14 CEs (o agrupadas por familia)

## Tareas

1. Scaffold `.storybook` bajo `libs/base/frontend/crosscutting/native-ui/`.
2. Registrar CEs en preview (`registerNativeUi()`).
3. Targets Nx en `project.json` / inferred: `storybook`, `build-storybook`
   (puerto p. ej. **4400** — “home” del design system).
4. Stories mínimas: variantes size/tone/disabled por elemento clave.
5. Script root `pnpm storybook:native-ui`.
6. CI opcional: `build-storybook` de `base-native-ui` en job frontend (o
   soft primero).
7. Enlazar desde ui-strategy § Storybook: **native primero**, wrappers después.

## Criterios de aceptación

- [ ] `pnpm nx storybook base-native-ui` sirve el catálogo Lit.
- [ ] `pnpm nx build-storybook base-native-ui` produce `dist/storybook/…`.
- [ ] Al menos stories para button/input/alert + lista del resto (o CSF
      autoload).
- [ ] Doc de puerto y comando en design-system / guides.
