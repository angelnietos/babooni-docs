<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F81-D1 — Retire legacy Arq* CSS / dual atoms</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F81" src="https://img.shields.io/badge/round-F81-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24

## Objetivo

Eliminar el dualismo `atoms/button/ArqButton*` (premium CSS) vs `ArqNativeButton*` (Lit) en brand UI. Un solo camino visual: **native SoT**.

Regla: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md) — no wrappers vacíos; si el “premium” no aporta marca real sobre Lit+tokens, **borrar** o dejar re-export thin de `ArqNative*`.

## Acciones

- [x] Inventario `libs/arquetipos/frontend/{angular,react}/ui/src/lib/atoms/**` vs refs en features/Storybook
- [x] Por átomo: DELETE CSS dual (button/input/alert/badge/card/modal/select/skeleton/spinner/icon); KEEP `ArqNative*`
- [x] Actualizar Storybook brand: historias canónicas `ArqNativeButton`
- [x] Quitar exports legacy del barrel (indexes solo native); apps `ArqSpinner` → `ArqNativeSpinner`
- [x] Actualizar [f80-dead-atom-inventory.md](../../../frontend/f80-dead-atom-inventory.md)

## Criterios

- [x] Un solo componente “Button” canónico por runtime en plantilla (`ArqNativeButton*`)
- [x] Typecheck brand UI verde
