<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F82-A1 — Inventario native ↔ Josanz</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F82" src="https://img.shields.io/badge/round-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

**hecho** · 2026-07-24

## Objetivo

Publicar una matriz de decisión por componente de `@josanz/angular-ui` frente a `@base/native-ui` / `@base/angular-ui`, **sin mover archivos**. Alimenta B1–D1.

## Clasificación

| Etiqueta | Significado |
|----------|-------------|
| **PROMOTE-TO-BASE** | Comportamiento genérico reutilizable → enriquecer Lit SoT (+ adapter Angular) |
| **WRAP** | SoT cubre el núcleo; Josanz aporta brand / defaults / template ERP |
| **RE-EXPORT** | Sin valor de marca aún → re-export `Native*` / base (YAGNI) |
| **JOSANZ-ONLY** | Solo producto (catalog, settings panels, delete host, Figma shells, …) |

Referencia: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md).

## Matriz publicada

→ **[josanz-native-inventory.md](../../../frontend/josanz-native-inventory.md)**

Canarios WRAP (orden): button → input → select → checkbox → spinner → badge → alert → modal.

B1 promotes canario: alert dismissible/neutral, badge size/soft/solid/dot, button outline+xl, checkbox description, modal sheet, `base-textarea`.

## Acciones

- [x] Inventariar átomos SoT actuales en `@base/native-ui` (tags + adapters `Native*`)
- [x] Listar componentes públicos de `@josanz/angular-ui` (barrel `src/index.ts` + carpetas `forms|feedback|actions|…`)
- [x] Asignar etiqueta a cada fila; documentar gaps que bloquearían WRAP (props, slots, eventos)
- [x] Priorizar canarios WRAP: button, input, select, alert, modal, badge, checkbox, spinner
- [x] Publicar matriz en anexo `docs/frontend/josanz-native-inventory.md`

## Criterios

- [x] Matriz revisable (tabla) con ≥ canarios + sample JOSANZ-ONLY
- [x] Ningún move físico en A1
- [x] Decisiones alineadas con “paridad visual app actual”
