# F50-A1 — Cobertura typecheck en apps y libs frontend

## Estado

completado

## Origen

Carry de [F49-A3](../plans-49-forty-nine-round/1750000042000-f49-frontend-typecheck-coverage.md).

## Resultado

- Libs base FE verdes: `base-angular`, `base-angular-api|ui|store`, `base-react`,
  `base-react-api|ui|store`, `base-native-ui`, `base-shared-store`.
- Apps verdes: `angular-single`, `react-single`, `next-single`, `josanz`,
  `document-generator`, `react-native-single`, `ionic-single`,
  **`verifactu-platform`** (fix: `tsconfig.spec.json` → `moduleResolution: "bundler"` /
  `module: "ES2022"`; el `node10` previo rompía `@angular/common/http` y
  `rxjs-interop`).
- `pnpm check:exports-paths` OK (365 packages, 0 errors).

## Criterios de aceptación

- [x] Typecheck verde en la lista objetivo.
- [x] `pnpm check:exports-paths` pasa.
- [x] Cero `TS2307` por barrels rotos en esos proyectos.
