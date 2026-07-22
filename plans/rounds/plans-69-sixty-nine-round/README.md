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

> Cierre 2026-07-23. Eje: **harden `presentation:auto`** (CSS SoT + e2e);
> **Ionic FeatureShell** rollout users/roles; **retire alias** `clients-list*`;
> carries Chromatic/Zod **defer F70**; board piloto **defer F70**.

## Contexto

F68 entregó Native board CE, retiró dual `arq-clients*`, `ArqIonFeatureShell`
(piloto clients), users entity-view wire, y deferió Chromatic/Zod → F69.
Post-F68 apareció listado duplicado cards+table en Ionic clients (auto CSS
fuera de SoT / encapsulación) — fix en cierre F68 + e2e; F69 endurece.

| Deuda / gap | Plan F69 | Resultado |
|-------------|----------|-----------|
| `presentation:auto` SoT + e2e multi-viewport | A1 | hecho |
| Ionic FeatureShell → users / roles / audit | A2 | hecho (users + roles) |
| Board piloto ≥1 dominio (no solo story) | A3 | defer F70 |
| Chromatic / Code Connect / atoms `@deprecated` | B1 | defer F70 |
| Zod kit opcional (ADR 0012) | B2 | defer F70 |
| Path alias `clients-list*` delete cuando residual=0 | B3 | hecho |
| Docs + hub F69 | C1 | hecho |

**Contrato:**

1. `data-presentation="auto"` nunca muestra cards y table a la vez.
2. Ionic list domains usan `ArqIonFeatureShell` (sin MobileX/DesktopX).
3. Board tiene piloto de dominio o defer F70 con owner.
4. Carries B*: hecho **o** defer F70 con motivo + owner.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F69-A1 | [FeatureShell auto presentation harden + e2e](1750000260000-f69-feature-shell-auto-e2e.md) | completado |
| F69-A2 | [Ionic FeatureShell rollout](1750000261000-f69-ionic-feature-shell-rollout.md) | completado (users + roles) |
| F69-A3 | [Board domain pilot](1750000262000-f69-board-domain-pilot.md) | defer F70 (owner: design-system / FE platform) |
| F69-B1 | [Carry: Chromatic / deprecated](1750000263000-f69-carry-chromatic-deprecated.md) | defer F70 (owner: design-system) |
| F69-B2 | [Carry: Zod kit](1750000264000-f69-carry-zod-kit.md) | defer F70 (owner: platform shared; blocker: capacité + DTO drift) |
| F69-B3 | [Retire clients-list path alias](1750000265000-f69-retire-clients-list-path.md) | completado |
| F69-C1 | [Docs polish + hub F69](1750000266000-f69-documentation-polish.md) | completado |

## Criterios de aceptación (ronda)

- [x] A1: SoT CSS + e2e desktop/narrow sin listado duplicado. 
- [x] A2: ≥2 dominios Ionic adicionales en `ArqIonFeatureShell`. 
- [x] A3: board piloto dominio → defer F70 (owner design-system / FE platform).
- [x] B1: Chromatic / deprecated → defer F70 (owner design-system / FE platform).
- [x] B2: Zod kit → defer F70 (owner platform shared; blocker: capacité + DTO drift).
- [x] B3: Alias path `clients-list*` retirado.
- [x] C1: hubs / residual notes / agent mirrors actualizados.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Sustituir class-validator en Nest por Zod.

## Predecesora / siguiente

- Cerrada: [F68](../plans-68-sixty-eight-round/)
- Siguiente: [F70](../plans-70-seventy-round/)
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
