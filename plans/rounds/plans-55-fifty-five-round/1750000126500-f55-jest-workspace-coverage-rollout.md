# F55-C3 — Jest workspace: preset + coverage en libs/apps

## Estado

listo para ejecutar

## Objetivo

Extender la **config Jest global** ya aterrizada (`jest.shared.cjs`,
`jest.preset.js`, `jest.preset.angular.cjs`, scripts `test:coverage*`, merge →
`coverage/global/`) al resto de **libs** y **apps** con `test` target (~111
proyectos), sin romper Angular (`jest-preset-angular`), caches Nx ni el gate BE
(`test:cov:check` / F55-C1).

**Por qué:** Jest es el runner único cross-framework (Nest / Angular / React /
isomórfico). Hoy muchos `jest.config.*` no usan el preset compartido ni escriben
coverage bajo `coverage/<projectRoot>/`, así que el merge global y el cache de
outputs quedan incompletos.

## Contexto (ya hecho — no rehacer)

| Artefacto | Rol |
|-----------|-----|
| `jest.shared.cjs` | cache `node_modules/.cache/jest`, reporters, `collectCoverageFrom` |
| `jest.preset.js` | `@nx/jest/preset` + shared (React / Node / isomórfico) |
| `jest.preset.angular.cjs` | `jest-preset-angular` + shared |
| `pnpm test:coverage*` | `nx … test -- --coverage` + `merge-jest-coverage.mjs` |
| Pilotos | `base-ui-tokens`, `base-native-ui`, `base-react-ui`, `base-angular-ui`, arquetipos UI |
| Docs | [testing-pyramid.md](../../../guides/testing-pyramid.md), [jest-coverage.md](../../../runbooks/jest-coverage.md) |

## Arquitectura (no violar)

| Capa | Dónde viven los tests unit | Apps |
|------|----------------------------|------|
| Dominio / handlers / UI | `libs/**` (`*.spec.ts(x)`) | Thin shells: prefer smoke mínimo o `passWithNoTests` |
| Integration | `*.int-spec.ts` + configs dedicadas | No mezclar en unit coverage workspace |
| E2E | Playwright `*-e2e` | Fuera de este plan |

- **Apps** = composition roots — no exigir coverage alto de bootstrap; el valor está en libs.
- **Angular** libs: `workspaceJestDefaults({ preset: 'jest-preset-angular', … })` o
  `preset: '<rel>/jest.preset.angular.cjs'` + overrides de transform/setup.
- **React / Node**: `preset: '<rel>/jest.preset.js'` (+ moduleNameMapper Lit/React si aplica).
- Coverage por proyecto: `coverage/<projectRoot>/` (incluye `coverage-final.json` vía reporter `json`).
- Merge: `pnpm test:coverage:merge` → `coverage/global/` (no cacheable).

## Tareas

### Inventario y oleadas

1. Inventariar `jest.config.*` bajo `libs/` y `apps/` que **no** referencian
   `jest.preset.js` / `jest.shared.cjs` / `jest.preset.angular.cjs`.
2. Clasificar en oleadas (orden sugerido):
   - **O1** — UI + crosscutting (`type:ui`, native-ui, shared-store) restantes.
   - **O2** — `*-data-access` / `*-api` base + arquetipos.
   - **O3** — backends (`@base/backend` ya OK; josanz/saas/apps Nest).
   - **O4** — apps FE thin (`react-single`, `angular-*`, `josanz`, SaaS shells):
     smoke o `passWithNoTests`; no inventar suites vacías.
3. Por proyecto: `tsconfig.spec.json` con `"types": ["jest", "node"]`;
   `coverageDirectory` vía shared (no paths relativos rotos).

### Tooling / CI (soft)

4. Script opcional `tools/scripts/check-jest-preset.mjs` (warn → strict en F56):
   configs con `test` target deben heredar shared o documentar exclusión.
5. CI (job `quality` o nightly): soft
   `pnpm test:coverage:affected && pnpm test:coverage:merge` + artifact
   `coverage/global/` — **no** fail umbral workspace en F55 (C1 es solo BE domains).
6. No bajar umbrales de `test:cov:check` (F55-C1).

### Docs / agentes (sync)

7. Mantener alineados: runbook Jest, testing-pyramid, ci-gates, `AGENTS.md`,
   `platform-bible` (`.opencode` → `.cursor` / `.github`), `CONTRIBUTING.md`.
8. Actualizar `.opencode/skills/nx-import/references/JEST.md` (preset = shared).

## Criterios de aceptación

- [ ] ≥70% de proyectos con target `test` usan shared preset **o** exclusión
      documentada en el Resultado del plan.
- [ ] `pnpm test:coverage:affected` + `test:coverage:merge` verdes en un PR
      canario (artifact `coverage/global` opcional en CI soft).
- [ ] Apps thin sin suites inventadas; Angular no regresa a doble React/Lit load.
- [ ] Nx cache hit en `nx test <proj>` sin cambios (inputs incluyen
      `jest.preset.js` / `jest.shared.cjs`).
- [ ] Docs + platform-bible sincronizados; `pnpm test:cov:check` intacto.

## Riesgos

- `process.cwd()` en shared asume `cwd` = project root (plugin `@nx/jest`) —
  jest directo desde raíz escribe en `coverage/root` (OK / documentado).
- Comentarios JSDoc con `/**/` en globs rompen parsers ESM — evitar.
- Dual React en Jest: pin workspace root (ver `base-react-ui` jest.config).
- Coverage bajo en native-ui (contratos fuente) — no inventar umbrales UI aún.

## Relación

| Plan | Relación |
|------|----------|
| F55-C1 | Gate BE domains strict — independiente; no mezclar umbrales |
| F55-E1 | Cierre docs si C3 aterriza parcialmente |
| F56 | `check-jest-preset --strict` + umbrales workspace opcionales |
