<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F66 — SoC features + vistas genéricas + carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Abierta 2026-07-22. Eje: **SOLID / separación de responsabilidades** en features
> arquetipos (lógica fuera de components; `layout/` · `pages/` · `components/`
> en todas las libs features), **abstracciones de vista padre** (read / write por
> propiedad), y carries F65 (Chromatic·deprecated, Zod kit, ESLint store ratchet).

## Contexto

F65 cerró confirm+toast multi-stack, ADR 0012 (predicates clients), facade SoC
clients (D1) y eliminó dual stores demo. Quedan: deuda visual Chromatic /
atoms `@deprecated`, kit Zod opcional, y endurecer SoC **dentro** de features
(no solo data-access).

| Problema | Plan F66 |
|----------|----------|
| Lógica de negocio / CRUD en pages-components; gaps `layout/pages/components` | A1 |
| Vistas CRUD duplicadas; falta padre genérico read/write por campo | A2 |
| Chromatic / Code Connect / deprecated atoms (F65-B2) | B1 |
| Zod kit isomórfico (F65-B1 defer) | B2 |
| ESLint features ↔ store concreto (F65-D1) | B3 |
| Docs + hub F66 / cierre F65 | C1 |

**Contrato:**

1. Features **no** concentran reglas de dominio ni orquestación HTTP en el
   template/JSX — van a **servicios** (Angular/Ionic) o **hooks** (React/Next/RN)
   en `features/src/services|hooks/` o en data-access facade.
2. Toda lib `*-features` (base + arquetipos, web + mobile) expone
   `layout/` · `pages/` · `components/` (+ routes). Gate:
   `check-frontend-conventions` (ampliar a mobile/Next si falta).
3. Formularios / detalle reutilizan **vista padre** configurable: modo
   `read` \| `write` \| `mixed` y flags por propiedad (solo lectura / solo
   escritura / hidden).
4. Carries B*: hecho **o** defer F67 con motivo + owner — no “listo eterno”.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F66-A1 | [Features SoC + layout/pages/components](1750000230000-f66-features-layout-soc.md) | listo para ejecutar |
| F66-A2 | [Entity view abstractions (read/write)](1750000231000-f66-entity-view-abstractions.md) | listo para ejecutar |
| F66-B1 | [Carry: Chromatic / deprecated](1750000232000-f66-carry-chromatic-deprecated.md) | listo para ejecutar |
| F66-B2 | [Carry: Zod kit](1750000233000-f66-carry-zod-kit.md) | listo para ejecutar |
| F66-B3 | [ESLint features↔store ratchet](1750000234000-f66-eslint-features-store-ratchet.md) | listo para ejecutar |
| F66-C1 | [Docs polish + hub F66](1750000235000-f66-documentation-polish.md) | listo para ejecutar |

## Criterios de aceptación (ronda)

- [ ] A1: piloto ≥2 dominios × ≥2 stacks con lógica fuera del component; gate
      layout/pages/components verde en features arquetipos (+ base).
- [ ] A2: API padre documentada + piloto clients (detalle/form) con modo read y
      write por propiedad.
- [ ] B1–B3: hecho **o** defer F67 explícito.
- [ ] C1: hubs → F66; F65 cerrada; guías + agent mirrors al día.

## Fuera de alcance

- Redesign Josanz/SaaS product UX (salvo consumo de abstracciones `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo (Zod no reemplaza class-validator Nest).
- Migrar auth session stores a facade (sigue en auth data-access).
