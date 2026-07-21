# F51-C2 — Higiene artefactos + ignore `tsbuildinfo`

## Estado

completado

## Objetivo

Evitar que artefactos de typecheck (`*.tsbuildinfo`) y similares entren en
commits / PRs. Alinear `.gitignore` y limpiar tracked si aplica.

## Resultado

- `.gitignore`: `*.tsbuildinfo` + `**/*.tsbuildinfo`.
- Untracked del índice: `apps/arquetipos/frontend/nextjs/next-{single,multi}/tsconfig.tsbuildinfo`
  (`git rm --cached`).
- `git ls-files '*.tsbuildinfo'` → vacío.

## Criterios de aceptación

- [x] Ningún `*.tsbuildinfo` tracked.
- [x] `.gitignore` cubre el patrón.
