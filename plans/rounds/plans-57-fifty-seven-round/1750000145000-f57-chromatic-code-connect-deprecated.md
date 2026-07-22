<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F57-D1 — Carry F56: Chromatic / Code Connect / remove deprecated</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

defer F58

## Objetivo

Cerrar o re-diferir con motivo los tres carries de F55/F56:

1. **Chromatic** — visual regression Storybook (token `CHROMATIC_PROJECT_TOKEN`).
2. **Figma Code Connect** — mapear primitivos native-ui / wrappers.
3. **Remove `@deprecated`** — primitivos framework-only marcados en F54.

## Criterios de aceptación

- [ ] Cada ítem: **hecho** en F57 **o** defer F58 con bloqueo explícito (sin token / sin acceso Figma / riesgo de breaking).
- [ ] Si Chromatic: job soft o strict documentado; no romper PR sin token.
- [ ] Si remove deprecated: codemod o PR acotado + `check:deprecated` / native-first.
- [ ] Sin regresiones `check:ui-ownership:strict`.

## Resultado

**Defer F58:** sin `CHROMATIC_PROJECT_TOKEN` ni Figma Code Connect en CI.
Remove de primitivos `@deprecated` no ejecutado. `build-storybook` base UI
sigue como señal de rotura de stories.
