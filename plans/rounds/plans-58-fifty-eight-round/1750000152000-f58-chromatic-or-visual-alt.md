<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F58-B1 — Chromatic o alternativa visual (carry F57-D1)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F59 (sin CHROMATIC_PROJECT_TOKEN)

## Objetivo

Cerrar el carry de visual regression Storybook: Chromatic **o** alternativa
(Loki / Playwright SB screenshots) con job soft/nightly.

## Criterios de aceptación

- [x] Chromatic cableado con `CHROMATIC_PROJECT_TOKEN` **o** alternativa documentada **o** defer F59 con bloqueo (sin token).
- [x] No fallar PR sin secreto configurado (soft / skip).
- [x] `design-system.md` + `ci-gates.md` actualizados.

## Notas

Sin secretos en git. Prefer soft/nightly hasta baseline estable.
