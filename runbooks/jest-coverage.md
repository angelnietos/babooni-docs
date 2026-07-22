# Jest + coverage (workspace)

Cuándo usarla: configurar o depurar unit tests Jest, coverage por proyecto o
informe global; alinear un `jest.config.*` nuevo al preset compartido.

## Idea

Jest es el **único** runner unit cross-framework (Nest, Angular, React,
isomórfico). Nx (`@nx/jest/plugin`) infiere `test` desde `jest.config.*`.

| Archivo | Uso |
|---------|-----|
| `jest.shared.cjs` | Defaults: cache, reporters, `coverageDirectory` (sin forzar inventario) |
| `jest.preset.js` | React / Node / isomórfico (`@nx/jest/preset` + shared) |
| `jest.preset.angular.cjs` | Angular (`jest-preset-angular` + shared) |
| `tools/jest/merge-jest-coverage.mjs` | Une `coverage-final.json` → `coverage/global/` |

**Qué aparece en el HTML de coverage:** por defecto **solo módulos cargados** durante
la corrida. Un `collectCoverageFrom: ['src/**']` amplio mete todos los `.ts` en el
informe (filas a **0%** aunque los CEs Lit estén mockeados). Por eso el shared
**no** define `collectCoverageFrom`; los gates BE usan su propio globs en
`libs/base/backend/jest.config.cts`. Opt-in inventario: `OPT_IN_COLLECT_COVERAGE_FROM`
en `jest.shared.cjs`.

**Salida por proyecto:** `coverage/<projectRoot>/` (lcov, html, json, json-summary).  
**Global:** `coverage/global/` tras el merge.  
**Cache Jest:** `node_modules/.cache/jest`.  
**Cache Nx:** target `test` cacheable; inputs incluyen los presets del root.

**Adopción preset (F55-C3):** `pnpm check:jest-preset` (warn) /
`pnpm check:jest-preset:strict`. Exclusión documentada:
`libs/base/backend/jest.config.cts` (umbrales dominio gate C1) o comentario
`// jest-preset: exclude`.

**Coverage backend workspace (F56-B1):** `pnpm test:coverage:backend` —
`nx run-many -t test -p tag:runtime:backend -- --coverage` (+ merge opcional).
No sustituye ni baja `pnpm test:cov:check` (dominios audit/settings/tenants).

**Verdad del informe (F57 soft → F58-A1 strict):**

```bash
pnpm nx test <project> -- --coverage
pnpm test:coverage:merge                 # → coverage/global/ + projects.json
pnpm report:coverage-inventory           # → coverage/inventory.json
pnpm check:coverage-truth                # soft
pnpm check:coverage-truth:strict         # CI quality (F58-A1)
pnpm check:coverage-thresholds           # F58-A2 soft opt-in por proyecto
```

Cada proyecto debe emitir bajo `coverage/<projectRoot>/` (vía `nx test`). Evitar
`jest -c` desde la raíz (`coverage/root`). El gate BE `test:cov:check` sigue
aparte (umbrales audit/settings/tenants).

Inventario intencional (`collectCoverageFrom` amplio): marcar el config con
`// coverage-truth: allow-broad-collect` (apps backend + roots arquetipos FE).
Sin el marcador, `--strict` falla.

### Cómo leer el informe (F58-A2)

| Capa / señal | Qué mirar | Gate |
|--------------|-----------|------|
| Proyecto unit | `coverage/<projectRoot>/` HTML / lcov | Local / affected |
| Merge workspace | `coverage/global/` | Soft CI (`test:coverage:affected` + merge) |
| Verdad artefactos | missing `coverage-final`, `coverage/root`, broad collect | `check:coverage-truth:strict` |
| BE dominios | audit / settings / tenants | `test:cov:check` (**fail**) |
| Opt-in % lines | allowlist en `check-coverage-thresholds.mjs` | soft local; no umbral monorepo único |

No hay un % único para todo el monorepo. Umbrales por capa = opt-in (A2) o gate BE.


## Apps vs libs

| | Libs | Apps |
|---|------|------|
| Unit tests | Sí — dominio, UI, data-access | Smoke mínimo o `passWithNoTests` |
| Coverage útil | Sí | Bajo valor en bootstrap; no inventar suites vacías |
| Integration | `*.int-spec.ts` configs aparte | — |

Plan de rollout: [F55-C3](../plans/rounds/plans-55-fifty-five-round/1750000126500-f55-jest-workspace-coverage-rollout.md).

## Plantillas de config

### React / Node / isomórfico

```ts
import type { Config } from 'jest';

export default {
  displayName: 'my-lib',
  preset: '../../../../jest.preset.js', // ajustar profundidad al root
  testEnvironment: 'jsdom', // o 'node'
  // overrides: moduleNameMapper, setupFilesAfterEnv, …
} satisfies Config;
```

### Angular (zona / TestBed)

```ts
import type { Config } from 'jest';
import { createRequire } from 'node:module';
import { dirname, join } from 'node:path';
import { fileURLToPath } from 'node:url';

const require = createRequire(import.meta.url);
const root = join(dirname(fileURLToPath(import.meta.url)), /* … */ '../../../../');
const { workspaceJestDefaults } = require(join(root, 'jest.shared.cjs'));

export default workspaceJestDefaults({
  displayName: 'my-angular-lib',
  preset: 'jest-preset-angular',
  setupFilesAfterEnv: ['<rootDir>/test-setup.ts'],
  // transform / moduleNameMapper como el resto de angular-ui
}) satisfies Config;
```

Alternativa: `preset: '<rel>/jest.preset.angular.cjs'` si no hace falta transform custom.

### Backend gate (dominios audit/settings/tenants)

Sigue `libs/base/backend/jest.config.cts` + `pnpm test:cov:check` (umbrales propios).
No mezclar con umbrales del merge workspace.

## Comandos

```bash
pnpm test:affected
pnpm test:coverage:affected          # Jest --coverage en affected
pnpm test:coverage:merge             # → coverage/global/
pnpm test:coverage:report:affected   # coverage + merge

# Gate BE (F51/F55-C1)
pnpm test:cov:check

# Proyecto concreto
pnpm nx test base-ui-tokens -- --coverage
```

`nx test` con `cwd` = project root → coverage en `coverage/<projectRoot>/`.  
Jest desde la raíz del repo con `-c path/to/config` → a menudo `coverage/root`
(documentado; preferir `nx test`).

## Pitfalls

| Síntoma | Causa / fix |
|---------|-------------|
| Merge: “no coverage-final.json” | Falta reporter `json` (viene en shared) o no corriste `--coverage` |
| HTML lleno de carpetas a **0%** | Había `collectCoverageFrom` amplio — shared ya no lo fuerza; regenera coverage |
| `react` / `react-dom` version mismatch | Pin dirs desde workspace root (ver `base-react-ui` jest.config) |
| Lit ESM en Jest CJS | Mock `@base/native-ui` / shim `registerNativeUi` (CEs no se ejecutan → no piden coverage) |
| Comentario `/**/` en globs | Cierra JSDoc ESM — no usar `**/` dentro de `/** … */` |
| Cache Nx no restaura coverage | `outputs` del target deben incluir `coverage/...` (plugin + `coverageDirectory`) |

## Verificación

```bash
pnpm nx test base-ui-tokens -- --coverage
pnpm nx test base-ui-tokens -- --coverage   # segundo run → cache hit
test -f coverage/libs/base/frontend/crosscutting/ui-tokens/coverage-final.json
pnpm test:coverage:merge
```

## Enlaces

- [testing-pyramid.md](../guides/testing-pyramid.md)
- [ci-gates.md](../frontend/ci-gates.md)
- [nx-daemon.md](./nx-daemon.md) — si `nx test` cuelga
- F55-C1 BE strict · F55-C3 rollout workspace
