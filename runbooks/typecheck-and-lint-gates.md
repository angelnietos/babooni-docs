# Typecheck + lint gates (DX)

**Desde:** F38 (`d0bbff49`, `c13f8f07`/`2e5650ac`) + F39-DX

## Typecheck incluye specs

El target `typecheck` (plugin `tools/typecheck/nx-typecheck-plugin.mjs`) invoca:

```bash
node tools/lib/run-typecheck.mjs <projectRoot>
```

Ese runner ejecuta `tsc --noEmit` sobre:

1. `tsconfig.lib.json` o `tsconfig.app.json` (producción)
2. `tsconfig.spec.json` si existe (tests)

Jest/`isolatedModules` **no** sustituye el typecheck de specs (p. ej. TS2307 en imports).

**Inherited exclude blind spot:** si `tsconfig.spec.json` extiende un lib/app config con
`exclude: **/*.spec.ts`, hay que **sobreescribir** `exclude` en el spec config (p. ej.
`["node_modules","dist"]`) o los specs quedan fuera del gate aunque aparezcan en `include`.
Validar: `node tools/checks/validate-tsconfig-spec-json.mjs` (113 archivos, exit 0).
Bulk fix seguro: `node tools/checks/fix-tsconfig-spec-exclude.mjs --write` (usa JSON.stringify).

### Preferencias locales

| Situación | Comando |
|-----------|---------|
| Gate affected (CI-like) | `pnpm verify:affected` (= `nx affected -t lint,typecheck,test --parallel=3`) |
| Scope base + arquetipos (paralelo + cache) | `pnpm typecheck:all:base-arquetipos` |
| Un scope | `pnpm typecheck:all:base` / `pnpm typecheck:all:arquetipos` |
| Proyecto concreto (debug) | `node tools/lib/run-typecheck.mjs <projectRoot>` |
| Inventario rojo | `node tools/typecheck/report-typecheck-failures.mjs` |

**No** usar bucles secuenciales sobre `run-typecheck.mjs` para inventarios de workspace — es lento y no usa cache Nx. Usar `nx run-many -t typecheck` (ver scripts `typecheck:all:*`).

**Angular feature libs** que extienden `tsconfig.lib.json` con `moduleResolution: "bundler"`: alinear
`tsconfig.spec.json` con `module: "ES2022"` + `moduleResolution: "bundler"` (ver
`libs/clientes/josanz/angular/events/features/tsconfig.spec.json`). Evita resolución rota de
`@angular/*` con `node10` y coincide con el IDE.

Si `module: "commonjs"` en specs (p. ej. backend Jest), usa `moduleResolution: "node10"` o `"node"`.

Incluir carpetas `testing/**` en `include` cuando haya helpers compartidos — si no, solo se
typecheckean al importarse y **fixtures sin tipo de retorno explícito** (`demoClient()` sin
`: Client`) dejan pasar mocks incompletos aunque el IDE marque TS2739 en el spec con
`signal<Client[]>()`.

Anti-patrones en specs (el gate **no** los detecta):

- `signal([demoClient()])` sin genérico — infiere el literal del helper, no `Client[]`.
- `Partial<T>[]` o `overrides = {}` sin anotar — relaja comprobaciones de campos requeridos.
- Objetos inline en `.set([{…}])` sin `signal<T[]>()` tipado.

Si no hay specs, no incluir `tsconfig.spec.json` o limitar `include` a `**/*.spec.ts` bajo la carpeta real (`lib/` vs `src/`).
Deps workspace usadas solo en specs deben declararse en el `package.json` del proyecto.

## `no-explicit-any` es error en producción

En `eslint.config.mjs`, `@typescript-eslint/no-explicit-any` es **error** en código de producción.
Queda **off** en `*.spec.ts` / `*.test.ts` / `*.int-spec.ts` (mocks).

`@typescript-eslint/no-unused-vars` es **error** en todo `*.ts`/`*.tsx` (specs incluidos).
Prefijos `^_` en args/vars/caughtErrors se ignoran (`argsIgnorePattern`, etc.). Antes era
**warn** vía preset Nx → `nx lint` exit 0 con warnings que el IDE mostraba como problemas.

**No** activar `maxWarnings: 0` aún — otras reglas (p. ej. dependency-checks) aún generan ruido.

Barrido F39-D3 (2026-07-19): producción en `libs/base/frontend/**` y `libs/arquetipos/**` sin `: any` / `as any`; ESLint directo (platform + domains + arquetipos frontend) exit 0. Otros errores lint (p. ej. `@nx/dependency-checks` en react features buildable) quedan fuera del gate `any`.

## CI

| Gate | Comando | Qué ejecuta |
|------|---------|-------------|
| PR / push (affected) | `pnpm verify:affected` | `nx affected -t lint,typecheck,test --parallel=3` |
| Push main (full typecheck) | `pnpm typecheck:all` | todos los scopes |
| Scope base+arquetipos | `pnpm typecheck:all:base-arquetipos` | inventario F39-DX |

Workflow: `.github/workflows/ci.yml` (jobs `verify` + `frontend`).

Checklist humano alineado con esos gates: [guides/pr-checklist.md](../guides/pr-checklist.md).

## Ver también

- [nx-daemon.md](./nx-daemon.md) — flakes del daemon
- [CONTRIBUTING.md](../../CONTRIBUTING.md) — `pnpm verify:affected`
- [guides/pr-checklist.md](../guides/pr-checklist.md) — PR ↔ CI
- [frontend/ci-gates.md](../frontend/ci-gates.md) — tabla de gates FE
