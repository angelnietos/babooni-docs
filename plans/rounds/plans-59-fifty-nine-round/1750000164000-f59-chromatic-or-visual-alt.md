<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F59-B1 — Chromatic o alternativa visual (carry F58-B1)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F60-E2 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Cerrar visual regression Storybook: Chromatic **o** Playwright screenshots sobre
SB static (soft/nightly), o defer F60 si sigue sin token/presupuesto.

## Criterios de aceptación

- [ ] Chromatic cableado con `CHROMATIC_PROJECT_TOKEN` **o** job soft PW SB
      **o** defer F60 documentado en `design-system.md` + `ci-gates.md`.
- [ ] PR sin secreto no falla en rojo duro.
