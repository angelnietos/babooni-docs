<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F67 — FeatureShell closeout + entity-view + visual/Zod carries</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

> Cerrada 2026-07-22. Eje: **FeatureShell adoption** (Angular/React wire;
> Ionic CSS-only intencional); **EntityView** en clients + roles + users;
> **ratchet** features↔store expandido; carries Chromatic / Zod → F68
> (board Native, alias `arq-clients*`).

## Contexto

F66 cerró SoC en features, FeatureShell `cards`/`table` + CSS `arq-feature*`,
facade parity users/roles/audit y check `features-no-store`. F67 cerró adopción
real del shell y rollout entity-view; deferió board Native y carries visual/Zod.

| Deuda / gap | Plan F67 | Resultado |
|-------------|----------|-----------|
| `presentation: 'board'` (Native board) | A1 | defer F68 |
| Shell Angular/React cableado; alias `arq-clients*` | A2 | hecho (alias remove → F68) |
| EntityView solo piloto clients | A3 | hecho (roles + users + clients slot) |
| Chromatic / Code Connect / atoms `@deprecated` | B1 | defer F68 |
| Zod kit opcional (ADR 0012) | B2 | defer F68 |
| `check-features-no-store --strict` solo clients | B3 | hecho (clients/users/roles/audit) |
| Docs + hub F67 | C1 | hecho → abre F68 |

**Contrato:**

1. FeatureShell es el único chrome de lista CRUD en arquetipos (`arq-feature*`).
2. Board es strategy de primera clase cuando Native board existe (F68-A1).
3. EntityViewConfig se reutiliza en ≥2 dominios × ≥2 stacks.
4. Features no importan stores concretos donde hay facade (ratchet expandido).
5. Carries B*: hecho **o** defer F68 con motivo + owner (no “listo” silencioso).

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F67-A1 | [FeatureShell board strategy](1750000240000-f67-feature-shell-board.md) | completado (defer F68) |
| F67-A2 | [FeatureShell adoption closeout](1750000241000-f67-feature-shell-adoption-closeout.md) | completado |
| F67-A3 | [Entity view rollout](1750000242000-f67-entity-view-rollout.md) | completado |
| F67-B1 | [Carry: Chromatic / deprecated](1750000243000-f67-carry-chromatic-deprecated.md) | completado (defer F68) |
| F67-B2 | [Carry: Zod kit](1750000244000-f67-carry-zod-kit.md) | completado (defer F68) |
| F67-B3 | [Features↔store ratchet expand](1750000245000-f67-features-store-ratchet-expand.md) | completado |
| F67-C1 | [Docs polish + hub F67](1750000246000-f67-documentation-polish.md) | completado |

## Criterios de aceptación (ronda)

- [x] A1: `presentation: 'board'` documentado + implementado **o** defer F68
      explícito (Native board no listo).
- [x] A2: ≥2 stacks montan `FeatureShell` real; alias `arq-clients*` deprecado
      con plan de retirada; CSS dual sin romper `@charset` / `@keyframes`.
- [x] A3: EntityView en ≥1 dominio adicional (users o roles) × ≥2 stacks;
      form/detail en slot del shell en ≥1 dominio.
- [x] B1–B3: hecho **o** defer F68 con owner/blocker.
- [x] C1: hubs / residual notes / agent mirrors al día.

## Fuera de alcance

- Redesign Josanz/SaaS (salvo consumo `@base/*`).
- Sustituir ADR 0012 sin ADR nuevo / addendum.
- Auth session store (sigue auth data-access).
- Sustituir class-validator en Nest por Zod.

## Siguiente ronda

[F68](../plans-68-sixty-eight-round/) — Native board CE; retire `arq-clients*`
alias; Ionic FeatureShell wrapper; Chromatic / Zod carries; entity-view deepen.
