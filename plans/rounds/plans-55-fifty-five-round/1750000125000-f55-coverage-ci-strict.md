<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F55-C1 — Coverage BE soft → strict</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Carry F52-B2 / F53-D1 / F54-D2: el job `pnpm test:cov:check` sigue
`continue-on-error`. Promover a **fail** cuando haya evidencia de estabilidad.

## Tareas

1. Contar PRs verdes post-F54 con soft gate (umbral sugerido: ≥5 o 2 semanas).
2. Quitar `continue-on-error` en `.github/workflows/ci.yml` **o** defer F56
   con métrica en Resultado.
3. Sync testing-pyramid / ci-gates docs.

## Criterios de aceptación

- [x] Strict activo **o** defer explícito con conteo/motivo.
- [x] Sin bajar umbrales para “pasar”.

## Resultado

Eliminado `continue-on-error` del job quality en `ci.yml`. `pnpm test:cov:check`
verde (~96% statements).
