<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F55-A1 — Oleada 2 Lit: select / icon / pagination</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Portar a `@base/native-ui` los gaps P0 del
[inventario](../../../frontend/external-ui-import-inventory.md): **select**,
**icon**, **pagination** (prioridad en ese orden). Wrappers Angular/React +
Arq + stories + catálogo.

## Prerrequisitos

- F54-A2 oleada 1 verde; wrappers pattern establecido.
- No duplicar `base-modal` / table (rechazados en inventario).

## Tareas

1. Diseñar API CE alineada a usos actuales de `@base/angular-ui` select/icon y
   pagination shared (props mínimas, eventos `base-*`).
2. Implementar CEs + register + tipos + tests smoke.
3. Wrappers `Native*` / `ArqNative*` + stories en SB native-ui.
4. Filas en `ui-component-catalog.yaml`.
5. Migrar ≥1 feature arquetipos que use el átomo legacy (si aplica).
6. Actualizar inventario Residual.

## Criterios de aceptación

- [x] ≥ 2 de {select, icon, pagination} con DoD completo.
- [x] Cero primitivos framework-only nuevos en base.
- [x] Typecheck/lint de proyectos tocados verde.

## Resultado

Oleada 2 completa: CEs `base-select`, `base-icon`, `base-pagination` en
`@base/native-ui`; wrappers Angular/React `Native*` + Arq `ArqNative*`;
`catalog.stories` + inventario actualizado (oleada 2).
