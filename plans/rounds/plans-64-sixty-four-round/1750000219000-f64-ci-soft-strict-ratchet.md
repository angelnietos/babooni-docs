<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-D2 — CI soft→strict ratchet</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Subir un escalón de calidad CI sin romper el monorepo: gates que hoy son
`continue-on-error` / soft → fail en canarios.

## Entregables

1. Elegir **uno** (mínimo) para promover:
   - `check:jest-preset` canario libs, **o**
   - `check:coverage-thresholds` en proyectos acordados, **o**
   - `mf-host-e2e` smoke estable en `main`.
2. Documentar en [ci-gates.md](../../../frontend/ci-gates.md) qué sigue soft.
3. No diluir umbrales BE (`test:cov:check`) ya strict.

## Criterios de aceptación

- [ ] Al menos un gate promovido a fail **o** motivo defer F65.
- [ ] `ci-gates.md` refleja la matriz actualizada.

## Verificación

Revisar `.github/workflows/ci.yml` + docs.

## Depende de

F64-B1 recomendado si el gate es mobile e2e.
