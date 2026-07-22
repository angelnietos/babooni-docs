<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F69 — Presentation harden + Ionic rollout + visual/Zod carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Apertura 2026-07-22. Eje: **harden `presentation:auto`** (CSS SoT + e2e);
> **Ionic FeatureShell** rollout users/roles/audit; **board piloto** dominio;
> carries **Chromatic / Code Connect / deprecated** + **Zod kit** (desde F68-B*).

## Contexto

F68 entregó Native board CE, retiró dual `arq-clients*`, `ArqIonFeatureShell`
(piloto clients), users entity-view wire, y deferió Chromatic/Zod → F69.
Post-F68 apareció listado duplicado cards+table en Ionic clients (auto CSS
fuera de SoT / encapsulación) — fix en cierre F68 + e2e; F69 endurece.

| Deuda / gap | Plan F69 |
|-------------|----------|
| `presentation:auto` SoT + e2e multi-viewport | A1 |
| Ionic FeatureShell → users / roles / audit | A2 |
| Board piloto ≥1 dominio (no solo story) | A3 |
| Chromatic / Code Connect / atoms `@deprecated` | B1 |
| Zod kit opcional (ADR 0012) | B2 |
| Path alias `clients-list*` delete cuando residual=0 | B3 |
| Docs + hub F69 | C1 |

**Contrato:**

1. `data-presentation="auto"` nunca muestra cards y table a la vez.
2. Ionic list domains usan `ArqIonFeatureShell` (sin MobileX/DesktopX).
3. Board tiene piloto de dominio o defer F70 con owner.
4. Carries B*: hecho **o** defer F70 con motivo + owner.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F69-A1 | [FeatureShell auto presentation harden + e2e](1750000260000-f69-feature-shell-auto-e2e.md) | listo para ejecutar |
| F69-A2 | [Ionic FeatureShell rollout](1750000261000-f69-ionic-feature-shell-rollout.md) | listo para ejecutar |
| F69-A3 | [Board domain pilot](1750000262000-f69-board-domain-pilot.md) | listo para ejecutar |
| F69-B1 | [Carry: Chromatic / deprecated](1750000263000-f69-carry-chromatic-deprecated.md) | listo para ejecutar |
| F69-B2 | [Carry: Zod kit](1750000264000-f69-carry-zod-kit.md) | listo para ejecutar |
| F69-B3 | [Retire clients-list path alias](1750000265000-f69-retire-clients-list-path.md) | listo para ejecutar |
| F69-C1 | [Docs polish + hub F69](1750000266000-f69-documentation-polish.md) | listo para ejecutar |

## Criterios de aceptación (ronda)

- [ ] A1: SoT CSS + e2e desktop/narrow sin listado duplicado.
- [ ] A2: ≥2 dominios Ionic adicionales en `ArqIonFeatureShell` **o** defer.
- [ ] A3: board piloto dominio **o** defer F70 (owner + blocker).
- [ ] B1–B3: hecho **o** defer F70 con owner/blocker.
- [ ] C1: hubs / residual notes / agent mirrors al día.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Sustituir class-validator en Nest por Zod.

## Predecesora / siguiente

- Cerrada: [F68](../plans-68-sixty-eight-round/)
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
