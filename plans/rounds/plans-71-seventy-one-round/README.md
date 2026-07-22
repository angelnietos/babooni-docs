<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F71 — Resolución carries F70 + expansión board</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Apertura 2026-07-23. Eje: **resolución carries F70** (Chromatic/Zod/deprecated);
> **expansión board piloto** (más dominios + e2e); **docs** al día.

## Contexto

F70 cerró board piloto (tasks), Ionic audit rollout, y docs polish. Quedaron deferidos:

| Deuda / gap | Plan F71 |
|-------------|----------|
| Chromatic CI soft + Code Connect | A1 |
| Migración auth + chrome → Native | A2 |
| Zod kit piloto / ADR 0013 addendum | B1 |
| Expandir board a más dominios + e2e | B2 |
| Docs + hub F71 | C1 |

**Contrato:**

1. Carries B* F70: hecho **o** defer F72 con owner/blocker explícito.
2. Board piloto: ≥2 dominios usan `presentation="board"` (tasks + roles/clients) **o** defer F72.
3. E2e smoke board pasa en desktop + narrow.
4. Docs actualizadas (hubs, residual notes, design-system/ci-gates si aplica).

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F71-A1 | [Carry: Chromatic / deprecated](1750000281000-f71-carry-chromatic-deprecated.md) | listo para ejecutar |
| F71-A2 | [Expand board pilot](1750000282000-f71-expand-board-pilot.md) | listo para ejecutar |
| F71-B1 | [Carry: Zod kit](1750000283000-f71-carry-zod-kit.md) | listo para ejecutar |
| F71-B2 | [Docs polish + hub F71](1750000284000-f71-documentation-polish.md) | listo para ejecutar |

## Criterios de aceptación (ronda)

- [ ] A1–A2: hecho **o** defer F72 con owner + blocker explícito.
- [ ] B1: Zod piloto **o** defer F72 (sin silencios).
- [ ] B2: hubs / residual notes actualizados a F71.

## Fuera de alcance

- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Sustituir class-validator en Nest por Zod.
- Redesign Josanz/SaaS (salvo consumo `@base/*`).

## Predecesora / siguiente

- Cerrada: [F70](../plans-70-seventy-round/)
- Siguiente: TBD
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
