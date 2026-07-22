<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F67 — FeatureShell closeout + entity-view + visual/Zod carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Apertura 2026-07-22. Eje: **cerrar FeatureShell** (board + adopción real de
> componentes / retirada de alias `arq-clients*`); **extender EntityView** más
> allá del piloto clients; **carries F66** (Chromatic·Code Connect·deprecated,
> Zod kit); **ampliar ratchet** features↔store a users/roles/audit.

## Contexto

F66 cerró SoC en features, FeatureShell `cards`/`table` + CSS `arq-feature*`,
facade parity users/roles/audit y check `features-no-store`. Quedó deferido:

| Deuda / gap | Plan F67 |
|-------------|----------|
| `presentation: 'board'` (Native board) | A1 |
| Shell Angular/React poco cableado; alias `arq-clients*`; dual-CSS frágil | A2 |
| EntityView solo piloto clients | A3 |
| Chromatic / Code Connect / atoms `@deprecated` | B1 |
| Zod kit opcional (ADR 0012) | B2 |
| `check-features-no-store --strict` solo clients-clean | B3 |
| Docs + hub F67 | C1 |

**Contrato:**

1. FeatureShell es el único chrome de lista CRUD en arquetipos (`arq-feature*`).
2. Board es strategy de primera clase cuando Native board existe.
3. EntityViewConfig se reutiliza en ≥2 dominios × ≥2 stacks.
4. Features no importan stores concretos donde hay facade (ratchet expandido).
5. Carries B*: hecho **o** defer F68 con motivo + owner (no “listo” silencioso).

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F67-A1 | [FeatureShell board strategy](1750000240000-f67-feature-shell-board.md) | listo para ejecutar |
| F67-A2 | [FeatureShell adoption closeout](1750000241000-f67-feature-shell-adoption-closeout.md) | listo para ejecutar |
| F67-A3 | [Entity view rollout](1750000242000-f67-entity-view-rollout.md) | listo para ejecutar |
| F67-B1 | [Carry: Chromatic / deprecated](1750000243000-f67-carry-chromatic-deprecated.md) | listo para ejecutar |
| F67-B2 | [Carry: Zod kit](1750000244000-f67-carry-zod-kit.md) | listo para ejecutar |
| F67-B3 | [Features↔store ratchet expand](1750000245000-f67-features-store-ratchet-expand.md) | listo para ejecutar |
| F67-C1 | [Docs polish + hub F67](1750000246000-f67-documentation-polish.md) | listo para ejecutar |

## Criterios de aceptación (ronda)

- [ ] A1: `presentation: 'board'` documentado + implementado **o** defer F68
      explícito (Native board no listo).
- [ ] A2: ≥2 stacks montan `FeatureShell` real; alias `arq-clients*` deprecado
      con plan de retirada; CSS dual sin romper `@charset` / `@keyframes`.
- [ ] A3: EntityView en ≥1 dominio adicional (users o roles) × ≥2 stacks;
      form/detail en slot del shell en ≥1 dominio.
- [ ] B1–B3: hecho **o** defer F68 con owner/blocker.
- [ ] C1: hubs / residual notes / agent mirrors al día.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Auth session store (sigue auth data-access).
- Sustituir class-validator en Nest por Zod.

## Predecesora / siguiente

- Cerrada: [F66](../plans-66-sixty-six-round/)
- Residual visual: [deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md)
- Residual validación: [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
