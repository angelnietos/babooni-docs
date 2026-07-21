# F51-B1 — Typecheck libs SaaS CRM (más allá de la app)

## Estado

completado

## Objetivo

F50 dejó verde `verifactu-platform` (app). Ampliar a libs
`libs/productos-saas/crm/frontend/angular/**` y backends CRM usados por la app
para que el canario no oculte deuda en packages.

## Resultado

**Batch:** `pnpm nx run-many -t typecheck -p "crm-*,verifactu-adapters,verifactu-ledger-core"`
(29 proyectos) → verde tras el fix abajo.

**Fix:** `crm-shared-browser-data-access` (`tsconfig.spec.json`) usaba
`moduleResolution: "node10"` + `module: "commonjs"`, incompatible con exports
de `@angular/common/http`. Alineado con F50 (`verifactu-platform`):
`moduleResolution: "bundler"`, `module: "ES2022"`.

**Nota:** otros `tsconfig.spec.json` CRM backend / worker siguen en `node10`
pero typecheckean OK (sin imports Angular package-exports). No tocados (YAGNI).

**Sin regresión:** `pnpm nx typecheck verifactu-platform` exit 0.

## Criterios de aceptación

- [x] Typecheck verde en lista objetivo (o exclusiones justificadas).
- [x] Sin regresión en `verifactu-platform` / `document-generator`.
