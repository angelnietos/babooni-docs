# Ronda F50 — Motor de empresa: typecheck FE + tests + calidad + DX docs

## Estado

en progreso (F50-B1 completado)

## Dependencias externas

- F49 cerrada (parcial): A1/A2/B1 hechos; A3/A4 → esta ronda.
- Biblia operativa ampliada (docs motor): hub, testing-pyramid, UI wrap, mobile/Next/Keycloak.
- F47 cerrada como superseded (F50-B1).

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F50-A1 | [Frontend typecheck coverage](1750000050000-f50-frontend-typecheck-coverage.md) | Carry F49-A3 |
| F50-A2 | [Tests backend faltantes](1750000051000-f50-add-missing-backend-tests.md) | Carry F49-A4 |
| F50-A3 | [Tipar int-specs / residual `as any` en tests](1750000052000-f50-cleanup-any-casts-in-int-specs.md) | Carry F49-A2 residual |
| F50-B1 | [Cerrar rondas zombie F47 (+ alinear F43)](1750000053000-f50-close-stale-rounds.md) | **completado** |
| F50-C1 | [DX mobile Metro + matriz puertos](1750000054000-f50-mobile-metro-and-ports-dx.md) | Motor opt-in |
| F50-C2 | [Gates CI ↔ biblia (PR checklist)](1750000055000-f50-align-ci-gates-with-bible.md) | Calidad |
| F50-D1 | [Documentación y cierre F50](1750000056000-f50-documentation.md) | Cierre |

## Objetivo

1. **Carry F49:** typecheck FE amplio + tests backend + tipado int-specs.
2. **Higiene planes:** cerrar F47 (superseded por F48/F49) y alinear estados.
3. **DX motor:** Metro React-18 pin documentado/compartido; puertos únicos.
4. **CI ↔ docs:** que `pr-checklist` y gates (`check:*`) estén alineados y visibles.
5. **Docs:** índice F50 + hub actualizado al cerrar.

## Orden de ejecución

1. F50-B1 (cerrar F47 zombie) — paralelo, sin bloquear código.
2. F50-A1 (FE typecheck) → batches libs → apps.
3. F50-A3 (int-specs any) — puede ir en paralelo a A2.
4. F50-A2 (tests backend faltantes).
5. F50-C1 (Metro / ports DX).
6. F50-C2 (CI ↔ biblia).
7. F50-D1 (docs cierre).

## Riesgos

- Typecheck FE masivo: mitigar por proyecto / `affected`.
- Tests nuevos frágiles: preferir harness + ports mock ([testing-pyramid.md](../../../guides/testing-pyramid.md)).
- Metro shared config: no romper Expo web en Windows.

## Criterios de aceptación

- [ ] `pnpm nx typecheck` verde en proyectos FE objetivo (lista A1).
- [ ] ≥ 10 tests nuevos; audit/settings/tenants ≥ 80% donde midamos.
- [ ] Int-specs domains sin `as any` grosero (o tipados con delegates).
- [ ] F47 marcada completada/superseded.
- [ ] Metro single/multi comparten helper o doc de pin React 18.
- [ ] Hub + plans README: F50 activa; F49 en archivo.
- [ ] `pnpm check:deprecated` + `pnpm check:exports-paths` pasan.
