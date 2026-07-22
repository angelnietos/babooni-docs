<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F66 — SoC + shell genérico + facade multi-dominio + carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Abierta 2026-07-22. Eje: **SOLID / SoC** en features; **shell de presentación
> genérico** (no chrome “clients”); **facade/state estándar en todos los
> dominios**; entity views read/write por campo; carries F65
> (Chromatic·deprecated, Zod, ESLint store).

## Contexto

F65 cerró confirm+toast, ADR 0012, facade clients y eliminó dual stores demo de
clients. Queda: same pattern para **users/roles/audit/…**, sustituir
`arq-clients*` compartido por un **FeatureShell** agnóstico, y SoC dentro de
features.

| Problema | Plan F66 |
|----------|----------|
| Lógica en pages-components; gaps `layout/pages/components` | A1 |
| Formularios/detalle: campos read/write sin duplicar (dominio-agnóstico) | A2 |
| Chrome `arq-clients*` copiado; falta shell genérico mobile/desktop/modes | A3 |
| Solo clients tiene facade; resto distinto / DEMO en page | D1 |
| Chromatic / Code Connect / deprecated atoms | B1 |
| Zod kit isomórfico | B2 |
| ESLint features ↔ store concreto | B3 |
| Docs + hub F66 | C1 |

**Contrato:**

1. Features: lógica en servicios/hooks/facade — no en templates.
2. Toda `*-features`: `layout/` · `pages/` · `components/` (+ routes).
3. **Presentación de lista** vía FeatureShell (A3) — slots + strategy
   `cards` \| `table` \| `board`; **no** layouts nombrados por dominio.
4. **Campos de entidad** vía EntityViewConfig (A2) — read/write/hidden.
5. **Datos** vía facade canónico por dominio (D1) — misma forma API en todos.
6. Carries B*: hecho **o** defer F67 con motivo + owner.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F66-A1 | [Features SoC + layout/pages/components](1750000230000-f66-features-layout-soc.md) | listo para ejecutar |
| F66-A2 | [Entity view abstractions (read/write)](1750000231000-f66-entity-view-abstractions.md) | listo para ejecutar |
| F66-A3 | [Generic feature shell (presentation)](1750000237000-f66-generic-feature-shell.md) | listo para ejecutar |
| F66-D1 | [Facade parity all domains](1750000236000-f66-domain-facade-parity.md) | listo para ejecutar |
| F66-B1 | [Carry: Chromatic / deprecated](1750000232000-f66-carry-chromatic-deprecated.md) | listo para ejecutar |
| F66-B2 | [Carry: Zod kit](1750000233000-f66-carry-zod-kit.md) | listo para ejecutar |
| F66-B3 | [ESLint features↔store ratchet](1750000234000-f66-eslint-features-store-ratchet.md) | listo para ejecutar |
| F66-C1 | [Docs polish + hub F66](1750000235000-f66-documentation-polish.md) | listo para ejecutar |

## Criterios de aceptación (ronda)

- [ ] A1: piloto ≥2 dominios × ≥2 stacks; gate layout/pages/components.
- [ ] A2: API padre domain-agnóstica + ≥1 dominio con read/write por campo.
- [ ] A3: FeatureShell genérico; ≥2 dominios sin chrome `arq-clients` semántico.
- [ ] D1: users + roles (+ audit) con facade estándar (no solo clients).
- [ ] B1–B3: hecho **o** defer F67 explícito.
- [ ] C1: hubs / guías / agent mirrors al día.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo de abstracciones `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo.
- Auth session store (sigue auth data-access).
