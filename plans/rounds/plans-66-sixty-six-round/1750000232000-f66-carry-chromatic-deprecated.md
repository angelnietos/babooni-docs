<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F66-B1 — Carry: Chromatic / deprecated atoms</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F66" src="https://img.shields.io/badge/round-F66-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Retomar [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
(F65-B2): Chromatic / Code Connect si hay secretos; migrar consumers residuales
de atoms `@deprecated` (auth + chrome) a `Native*` / `<base-*>`.

## Entregables

1. Chromatic CI soft **o** defer F67 (token + package).
2. Migración auth/login + presenters chrome (ErrorState, ConfirmDialog,
   SearchBar, SessionExpiry) → Native.
3. Actualizar inventario residual; `design-system.md` + `ci-gates.md`.

## Criterios de aceptación

- [x] Cada sub-ítem: hecho **o** defer F67 con owner/blocker en residual note.

## Verificación

`design-system.md` · `ci-gates.md` · `deprecated-atoms-residual.md`.

## Depende de

Secretos / Figma; no bloquea A1–A2.
