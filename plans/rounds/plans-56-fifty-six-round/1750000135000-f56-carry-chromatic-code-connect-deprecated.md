<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F56-D1 — Carry F55: Chromatic / Code Connect / remove deprecated</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

defer F57

## Objetivo

Resolver o re-diferir Chromatic, Code Connect y remove `@deprecated`.

## Criterios de aceptación

- [x] Cada ítem: hecho **o** defer F57 con motivo.
- [x] Sin regresiones ownership UI.

## Resultado

**Defer F57:** sin `CHROMATIC_PROJECT_TOKEN` ni acceso Figma Code Connect en CI.
Remove de primitivos `@deprecated` no ejecutado en esta ronda.
`build-storybook` base UI sigue como señal de rotura de stories.
