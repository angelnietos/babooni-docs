<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F70 — Board piloto + Chromatic/Zod carries + Ionic audit rollout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

> Apertura 2026-07-23. Eje: **board piloto** dominio (A1); **Ionic
> FeatureShell** rollout audit (A2); **Chromatic / deprecated** migration (B1);
> **Zod kit** opcional (B2); **docs** al día (C1).

## Contexto

F69 cerró `presentation:auto` harden + e2e, migró Ionic users/roles a
`ArqIonFeatureShell`, retiró alias `clients-list*`, y deferió a F70:

| Deuda / gap | Plan F70 |
|-------------|----------|
| Board piloto ≥1 dominio (no solo story) | A1 |
| Ionic FeatureShell → audit | A2 |
| Chromatic / Code Connect / atoms `@deprecated` | B1 |
| Zod kit opcional (ADR 0012) | B2 |
| Docs + hub F70 | C1 |

**Contrato:**

1. `presentation:auto` desktop/narrow sin listado duplicado (verificado en F69).
2. Ionic list domains usan `ArqIonFeatureShell` (users/roles/audit).
3. Board tiene piloto de dominio o defer F71 con owner.
4. Carries B*: hecho **o** defer F71 con motivo + owner.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F70-A1 | [Board domain pilot](1750000270000-f70-board-domain-pilot.md) | completado |
| F70-A2 | [Ionic FeatureShell rollout: audit](1750000271000-f70-ionic-feature-shell-audit.md) | completado |
| F70-B1 | [Carry: Chromatic / deprecated](1750000273000-f70-carry-chromatic-deprecated.md) | completado (defer F71) |
| F70-B2 | [Carry: Zod kit](1750000274000-f70-carry-zod-kit.md) | completado (defer F71) |
| F70-C1 | [Docs polish + hub F70](1750000276000-f70-documentation-polish.md) | completado |

## Criterios de aceptación (ronda)

- [x] A1: piloto dominio (`/tasks`) con `presentation="board"` + NativeBoard lanes.
- [x] A2: audit monta `ArqIonFeatureShell` y sin `MobileXPage`/`DesktopXPage`.
- [x] B1–B2: defer F71 con owner + blocker explícito (no silencios).
- [x] C1: hubs / residual notes actualizados.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Sustituir class-validator en Nest por Zod.

## Predecesora / siguiente

- Cerrada: [F69](../plans-69-sixty-nine-round/)
- Siguiente: F71
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
