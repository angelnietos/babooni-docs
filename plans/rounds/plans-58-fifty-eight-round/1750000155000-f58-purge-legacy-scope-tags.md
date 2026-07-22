# F58-C1 — Purge tags `scope:*` residuales

## Estado

completado

## Objetivo

Quitar `scope:angular|react|node|…` de `project.json` / package tags donde
`check-lib-layout --strict` ya exige solo `runtime:*` + `layer:*`.

## Entregables

1. Script one-off o edición batch en `libs/arquetipos` (+ muestra base/josanz si hace falta).
2. Verificar `pnpm check:lib-layout:strict`.
3. Confirmar scaffolds (F57) no reintroducen `scope:*`.

## Criterios de aceptación

- [x] 0 tags `scope:*` en libs arquetipos frontend (o allowlist documentada).
- [x] `check-lib-layout:strict` verde.
- [x] Sin romper `@nx/enforce-module-boundaries` (actualizar eslint depConstraints si aún citan scope).
