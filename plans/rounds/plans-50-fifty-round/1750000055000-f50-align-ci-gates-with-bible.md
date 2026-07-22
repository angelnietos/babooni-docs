<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F50-C2 — Alinear gates CI con la biblia (PR checklist)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

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
