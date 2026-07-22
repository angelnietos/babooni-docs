# Workspace packages — onboarding sin `tsconfig` paths (backend) / **con** paths (frontend)

Guía para añadir libs y consumir paquetes del monorepo.

> **Regla de oro (F16-PATHS):** los paquetes **frontend** (Angular/Vite, `runtime:angular` /
> `runtime:react`, bajo `frontend/`) consumidos por una app DEBEN tener una entrada `paths`
> **exacta** en `tsconfig.base.json` por cada entrada de `package.json` `exports` (incl.
> subpaths `./x`). Resueltos solo vía `exports` + symlink pnpm, el compilador Angular/Vite
> trata el `.ts` como externo y da "raw TS in browser" / "not found in TypeScript compilation".
> Los paquetes **backend** Node (Nest, `runtime:backend`, `exports` → `.js`) resuelven vía
> `exports` y **no** necesitan `paths`. Ver `check:exports-paths`.

## Cómo resuelve TypeScript hoy

1. Cada lib tiene `package.json` con `name`, `exports`, `types` y `main` → `./src/index.ts(x)`.
   (`pnpm enrich:packages` mantiene esto idempotente.)
2. Cada consumidor declara `"@scope/pkg": "workspace:*"` en `dependencies`.
   (`pnpm sync:workspace-deps` o `pnpm add --filter <consumer> --workspace`.)
3. `pnpm install` crea el symlink en `node_modules/@scope/pkg`.
4. `tsc` resuelve el import por el manifiesto del paquete — **no** hace falta un path en
   `tsconfig.base.json`.

### Excepciones (allowlist fija en `tsconfig.base.json`)

| Path | Motivo |
|------|--------|
| `@angular/core/testing` (+ platform-browser, router) | Shims de testing Angular |
| `@saas/prisma-client` | Cliente Prisma generado (workspace package en `libs/productos-saas/crm/backend/prisma-client`, `output` del schema) |

`@josanz/storybook` ya es paquete workspace (`libs/clientes/josanz/storybook`); importar
`@josanz/storybook/arg-types` vía `workspace:*`, sin path en tsconfig. Es tooling de
Storybook (no lo bundlea la app Angular/Vite), por eso no aplica la regla de `paths` de
F16-PATHS. Si algún día lo importa el bundle de la app, añadir su `paths`.

## Añadir una lib nueva

1. Generar la lib (Nx) bajo `libs/` con convención de capas (`api`, `data-access`, `shell`, `features`).
2. Asegurar `package.json`:
   ```json
   {
     "name": "@scope/mi-dominio-api",
     "version": "0.0.0",
     "private": true,
     "main": "./src/index.ts",
     "types": "./src/index.ts",
     "exports": { ".": { "types": "./src/index.ts", "default": "./src/index.ts" } }
   }
   ```
3. En cada consumidor que importe `@scope/mi-dominio-api`:
    ```bash
    pnpm add @scope/mi-dominio-api --filter @scope/consumidor --workspace
    ```
    o ejecutar `pnpm sync:workspace-deps:dry` / `pnpm sync:workspace-deps`.
4. `pnpm install`.
5. **Solo backend Node:** no añadir entrada en `tsconfig.base.json`.
   **Frontend (Angular/Vite):** añadir entrada `paths` por cada `exports` (`.` + subpaths):
   ```jsonc
   // tsconfig.base.json → compilerOptions.paths
   "@scope/mi-dominio-api": ["libs/<scope>/frontend/.../mi-dominio-api/src/index.ts"],
   "@scope/mi-dominio-api/sub": ["libs/<scope>/frontend/.../mi-dominio-api/src/lib/sub/index.ts"]
   ```
   Verificar con `pnpm check:exports-paths` (debe dar 0 errores en `--strict`).

## Apps consumidoras

Toda app bajo `apps/` debe tener `package.json` en la raíz del proyecto Nx (`project.json`).

Convención de nombres (Fase 4):

| Grupo | Patrón | Ejemplo |
|-------|--------|---------|
| `apps/arquetipos/*` | `@arquetipos/<leaf>` | `@arquetipos/angular-single` |
| `apps/clientes/<cliente>/*` | `@<cliente>/<leaf>` | `@josanz/josanz` (app web) |
| `apps/productos-saas/*` | `@saas/<leaf>` | `@saas/verifactu-crm-api` |

Colisión con lib homónima → sufijo `-app` (p. ej. `@josanz/backend-app`).

Las apps **no** necesitan path propio en `tsconfig.base.json`; declaran `workspace:*` hacia
los shells/features que lazy-loadan.

## Gates locales

```bash
pnpm check:workspace-deps:strict   # 0 violations — todo import @scope/* declarado
pnpm check:tsconfig-paths         # 0 missing/orphan (sin ratchet de cuenta; F16-PATHS)
pnpm check:exports-paths          # F16-PATHS: cada exports frontend tiene su paths
pnpm hygiene                      # layout + paths + workspace deps + exports/paths parity
```

## Verificación completa (post F9-BULK)

Tras el bulk removal (ratchet congelado en 4 paths), el typecheck global pasa:

| Scope | Comando | Resultado |
|-------|---------|-----------|
| Base | `pnpm typecheck:all:base` | 60 OK |
| Arquetipos | `pnpm typecheck:all:arquetipos` | 22 OK |
| SaaS | `pnpm typecheck:all:saas` | 29 OK |
| Afectados | `pnpm typecheck:affected` | 207 OK |

Ver también `pnpm typecheck:all:josanz` (capa cliente) y `docs/frontend/ci-gates.md`.

## Quitar paths de un dominio (oleada)

> **Precaución (F16-PATHS):** no quitar `paths` de un paquete **frontend** que consuma una
> app Angular/Vite. Resuelto solo por `exports`, el bundle da "raw TS" / "not found in
> TypeScript compilation". El impulso F9/F11 de "quitar paths" es la causa raíz de ese bug.
> Solo quitar paths si el paquete deja de ser consumido por el bundler (o pasa a backend Node).

Solo cuando el dominio está **confirmado con producto** (hoy: `events`, `clients`):

1. Verificar `workspace:*` en shell → features y app → shell.
2. `node tools/migrate/pilot-remove-tsconfig-paths.mjs --list` — revisar cluster.
3. Extender el script con el cluster, `--apply`, ratchet `--check-max` en `package.json`.
4. `pnpm check:tsconfig-paths` + `tsc` del cluster.

Dominios pendientes: revisar `tsconfig.base.json` para eliminar paths sobrantes.
Preview bulk: `pnpm pilot:remove-paths:bulk-dry`.

## ESLint / Nx boundaries

`workspace:*` **no sustituye** `@nx/enforce-module-boundaries`. El sync solo declara deps;
las reglas `layer:base` / `layer:clientes` / `layer:arquetipos` siguen vigentes.
