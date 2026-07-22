<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F68 — Native board + alias retire + visual/Zod carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

> Apertura 2026-07-22. Eje: **Native board CE** + FeatureShell `board`;
> **retirar alias** `arq-clients*` / dual CSS cuando consumers=0; **Ionic**
> FeatureShell wrapper (opcional); **carries F67** (Chromatic·Code Connect·
> deprecated, Zod kit); **entity-view deepen** (users form en pages; audit).

## Contexto

F67 cerró adopción FeatureShell (Angular/React), entity-view en roles/users/
clients slot, y ratchet features↔store. Quedó deferido:

| Deuda / gap | Plan F68 |
|-------------|----------|
| `presentation: 'board'` + Native board CE | A1 |
| Alias `arq-clients*` / dual CSS residual | A2 |
| Ionic CSS-only → thin FeatureShell wrapper | A3 |
| Chromatic / Code Connect / atoms `@deprecated` | B1 |
| Zod kit opcional (ADR 0012) | B2 |
| Entity-view deepen (users pages; audit) | B3 |
| Docs + hub F68 | C1 |

**Contrato:**

1. Board es strategy de primera clase cuando existe Native board CE.
2. `arq-feature*` es SoT; alias `arq-clients*` se retira al llegar a 0 consumers.
3. Ionic puede subir de CSS-only a thin wrapper sin duplicar MobileX/DesktopX.
4. EntityView se profundiza (wire form en pages; audit si aplica).
5. Carries B*: hecho **o** defer F69 con motivo + owner (no “listo” silencioso).

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F68-A1 | [Native board CE + FeatureShell board](1750000250000-f68-feature-shell-board.md) | completado |
| F68-A2 | [Retire arq-clients* alias / dual CSS](1750000251000-f68-retire-arq-clients-alias.md) | completado |
| F68-A3 | [Ionic FeatureShell thin wrapper](1750000252000-f68-ionic-feature-shell-wrapper.md) | completado |
| F68-B1 | [Carry: Chromatic / deprecated](1750000253000-f68-carry-chromatic-deprecated.md) | completado (defer F69) |
| F68-B2 | [Carry: Zod kit](1750000254000-f68-carry-zod-kit.md) | completado (defer F69) |
| F68-B3 | [Entity-view deepen](1750000255000-f68-entity-view-deepen.md) | completado |
| F68-C1 | [Docs polish + hub F68](1750000256000-f68-documentation-polish.md) | completado |

## Criterios de aceptación (ronda)

- [x] A1: Native board CE + `presentation: 'board'` **o** defer F69
      (owner + blocker).
- [x] A2: alias `arq-clients*` / dual CSS retirados cuando consumers=0 **o**
      inventario residual + defer.
- [x] A3: Ionic thin wrapper **o** CSS-only justificado en guía.
- [x] B1–B3: hecho **o** defer F69 con owner/blocker.
- [x] C1: hubs / residual notes / agent mirrors al día.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Auth session store (sigue auth data-access).
- Sustituir class-validator en Nest por Zod.

## Predecesora / siguiente

- Cerrada: [F67](../plans-67-sixty-seven-round/)
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
