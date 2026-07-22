<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F56-B1 — Extender Jest / coverage al backend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Profundizar coverage/scripts en backends Nest sin bajar `test:cov:check`.

## Criterios de aceptación

- [x] Script/documentación coverage BE workspace.
- [x] Coverage mergeable vía shared reporters.
- [x] `test:cov:check` sigue fail-on-threshold.

## Resultado

`pnpm test:coverage:backend` →
`nx run-many -t test -p tag:runtime:backend -- --coverage`.
Documentado en [jest-coverage.md](../../../runbooks/jest-coverage.md).
