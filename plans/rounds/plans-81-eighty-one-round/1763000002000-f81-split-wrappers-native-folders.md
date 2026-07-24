<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F81-B1 — Split `wrappers/native` → `atoms/{name}`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F81" src="https://img.shields.io/badge/round-F81-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24

## Objetivo

Dejar de tener **todos** los adapters amontonados en:

- `libs/arquetipos/frontend/angular/ui/src/lib/wrappers/native/`
- `libs/arquetipos/frontend/react/ui/src/lib/wrappers/native/`
- (espejo) `libs/base/frontend/{angular,react}/platform/ui/src/lib/wrappers/native/`

y pasar a **una carpeta por átomo**, alineada al contrato F80 / seed next-ionic-rn.

## Target

```text
src/lib/atoms/button/
  arq-native-button.component.ts   # angular brand
  ArqNativeButton.tsx              # react brand
  index.ts
src/lib/atoms/alert/
  …
src/lib/composites/confirm-dialog/
  arq-confirm-dialog.component.ts / ArqConfirmDialog.tsx
```

Base:

```text
@base/angular-ui  → lib/atoms/{atom}/native-*.component.ts
@base/react-ui    → lib/atoms/{atom}/Native*.tsx
```

## Acciones

- [x] Inventario archivo → átomo (button, input, alert, …; board → atoms/board o composites)
- [x] `git mv` arquetipos angular + react; actualizar barrels `src/index.ts` (API pública estable: mismos `ArqNative*` exports)
- [x] Misma oleada en `@base/angular-ui` / `@base/react-ui` `wrappers/native` (evitar drift brand↔base)
- [x] Borrar `wrappers/native/` (y `wrappers/` si queda vacío); actualizar docs que digan “adapters en wrappers/”
- [x] Extender `check-ui-lib-layout` si hace falta: fallar si reaparece `src/lib/wrappers/**/*.ts(x)` suelto o flat
- [x] Stories: mover junto al átomo; globs SB ya son `**/*.stories.*`
- [x] `tsc` / typecheck arquetipos-*-ui + base-*-ui

## Criterios

- [x] Cero archivos bajo `…/wrappers/native/` en arquetipos angular/react UI
- [x] Exports públicos sin breaking change de nombre
- [x] `pnpm check:ui-lib-layout:strict` verde
