<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F55-B2 — Smoke Playwright paridad Angular↔React (arquetipos)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

F54-B4 midió arquitectura + UI imports. Falta señal de **layout de página**
entre stacks Full (mismo flujo canario: auth → clients).

## Tareas

1. Spec Playwright soft: angular-single vs react-single (login demo + list
   clients empty/error/loading si estable).
2. Artifact screenshots opcional; no bloquear A1.
3. Enlazar desde `docs/arquetipos/parity.md`.
4. Si flaky: soft + issue; no fallar PR hasta estable.

## Criterios de aceptación

- [x] Spec soft en CI **o** defer justificado (“native-ui suficiente”).
- [x] Misma secuencia documentada en parity.md.

## Resultado

Misma secuencia smoke documentada en [parity.md](../../../arquetipos/parity.md).
`angular-single-e2e` + `react-single-e2e` `smoke.spec.ts` ya comparten
login → clients/users/audit.
