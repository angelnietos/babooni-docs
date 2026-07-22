<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F65 — Confirm + toast multi-stack + carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

> Cerrada 2026-07-22. Eje: **simetría UX de riesgo/feedback** (confirm modal +
> toast) en **todos** los arquetipos (Angular, React, Ionic, Next, RN), carries
> F64 (validación isomórfica / Chromatic·deprecated) y **pulido de estado / SoC
> front** (facade clients + guía).

## Contexto

F64 cerró portal select, Next listbox, mobile e2e, checkbox nativo y CI
`check:jest-preset` strict. Confirm+toast multi-stack cerró en A1–A4. B1 ADR
0012 + predicates clients; B2 Chromatic/deprecated → F66; D1 facade clients
en cinco stacks.

| Problema | Plan F65 |
|----------|----------|
| SPA Angular/React: verify + gaps residuales | A1 |
| Ionic clients/users/roles: sin confirm/toast | A2 |
| Next clients/users: sin primitives ni wiring | A3 |
| RN users/roles (+ confirm en clients) | A4 |
| Validación isomórfica (F64-D1 defer) | B1 |
| Chromatic / Code Connect / deprecated (F64-C2) | B2 |
| Docs + hub F65 / cierre F64 | C1 |
| Estado + SoC multi-stack (swap tool / tests) | D1 |

**Contrato:**

1. Acciones de riesgo (delete) → **confirm** antes de mutar.
2. Create / save / delete → **toast** `success` | `warning` | `danger`.
3. Implementación en **`libs/base/frontend/...`** cuando arquetipos es thin
   re-export (Ionic / Next / RN).
4. No Lit dentro de RN; Next **no** carga Lit CEs (webpack) — primitives
   React en `@base/next-ui`.
5. Features consumen **puertos/facades** de data-access; no stores concretos
   (D1).

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F65-A1 | [SPA Angular/React confirm+toast closeout](1750000221000-f65-spa-confirm-toast-closeout.md) | completado |
| F65-A2 | [Ionic confirm + toast CRUD](1750000222000-f65-ionic-confirm-toast.md) | completado |
| F65-A3 | [Next confirm + toast CRUD](1750000223000-f65-next-confirm-toast.md) | completado |
| F65-A4 | [RN confirm + toast CRUD](1750000224000-f65-rn-confirm-toast.md) | completado |
| F65-B1 | [Carry: validación isomórfica](1750000225000-f65-carry-isomorphic-validation.md) | completado |
| F65-B2 | [Carry: Chromatic / deprecated](1750000226000-f65-carry-visual-tooling.md) | completado (defer F66) |
| F65-C1 | [Docs polish + hub F65](1750000227000-f65-documentation-polish.md) | completado |
| F65-D1 | [Front state + SoC multi-stack](1750000228000-f65-frontend-state-soc.md) | completado |

## Criterios de aceptación (ronda)

- [x] Clients/users (y roles donde exista delete) en Angular, React, Ionic, Next, RN: confirm antes de borrar.
- [x] Mismas pantallas: toast en create/save/delete (warning validación).
- [x] B1: ADR 0012 + predicates clients; Zod kit defer F66.
- [x] B2: Chromatic / deprecated migration defer F66 (sin token; inventario actualizado).
- [x] Hubs → F65; F64 cerrada.
- [x] D1: piloto clients con facade en 5 stacks + guía + tests mockeables.

## Fuera de alcance

- Josanz/SaaS product UX (salvo consumo indirecto de `@base/*`).
- Lit en React Native o Next App Router.
- Rediseño total de shells.
- Check ESLint features↔store (warn→strict) — F66.
