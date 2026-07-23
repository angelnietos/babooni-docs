<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F72 — Cierre carries F71 + paridad scaffolding features</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Apertura 2026-07-23. Eje: **cierre carries F71** (Chromatic/Zod/board);
> **paridad scaffolding features** entre Angular / React / Next / Ionic / RN.

## Contexto

F71 cerró deferring carries a F72 y actualizó docs. Quedan pendientes los
mismos blockers estructurales más el gap de paridad física de features entre
stacks.

| Deuda / gap | Plan F72 |
|-------------|----------|
| Chromatic CI soft + Code Connect | A1 |
| Migración auth + chrome → Native atoms | A1 |
| Zod kit piloto / ADR 0013 addendum | B1 |
| Expansión board a otro dominio + e2e smoke | A2 |
| Paridad scaffolding features multi-framework | C1 |

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F72-A1 | [Carry: Chromatic / deprecated](1750000291000-f72-carry-chromatic-deprecated.md) | listo para ejecutar |
| F72-A2 | [Expand board pilot](1750000292000-f72-expand-board-pilot.md) | listo para ejecutar |
| F72-B1 | [Carry: Zod kit](1750000293000-f72-carry-zod-kit.md) | listo para ejecutar |
| F72-C1 | [Feature scaffold parity](1750000294000-f72-feature-scaffold-parity.md) | listo para ejecutar |

## Criterios de aceptación (ronda)

- [ ] A1–A2: hecho **o** defer F73 con owner + blocker explícito.
- [ ] B1: Zod piloto **o** defer F73 (sin silencios).
- [ ] C1: `check-frontend-conventions.mjs` pasa en base + arquetipos para
      Angular, React, Ionic; diferencias Next/RN documentadas o cerradas.

## Fuera de alcance

- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Sustituir class-validator en Nest por Zod.
- Rediseño de producto Josanz/SaaS.

## Predecesora / siguiente

- Cerrada: [F71](../plans-71-seventy-one-round/)
- Siguiente: TBD
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
