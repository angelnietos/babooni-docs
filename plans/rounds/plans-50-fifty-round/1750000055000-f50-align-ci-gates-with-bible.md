# F50-C2 — Alinear gates CI con la biblia (PR checklist)

## Estado

completado

## Resultado

- [pr-checklist.md](../../../guides/pr-checklist.md) espeja jobs `verify` /
  `frontend` / `quality`: `verify:affected`, `typecheck:all` (main),
  `check:schema-parity`, `check:domain-conventions:strict`,
  `check:frontend-conventions`, `check:lib-layout:strict`, `check:legacy-paths`,
  `check:node-modules`, `check:lint-coverage`, `check:slim-barrel`,
  `check:tsconfig-paths` + `check:exports-paths`, `check:workspace-deps:strict`,
  `check:ui-ownership:strict`, `check:deprecated`.
- Cross-links: [ci-gates.md](../../../frontend/ci-gates.md),
  [typecheck-and-lint-gates.md](../../../runbooks/typecheck-and-lint-gates.md).
- Comentario en `.github/workflows/ci.yml` → checklist.

## Criterios de aceptación

- [x] Checklist ↔ CI sin gates fantasma ni omitidos críticos.
- [x] Docs de gates FE/runbook enlazan el checklist.
