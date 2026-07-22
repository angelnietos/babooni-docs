<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">pnpm workspace layout (F8-DX1 / F8-DX5)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


## Linker

This monorepo uses **pnpm 10** with the default **`node-linker=isolated`**. Each
workspace package gets its own `node_modules/` with symlinks into the content-addressable
store (`.pnpm/`). That is **expected** under `libs/**/node_modules`.

### Forbidden nested installs

These break tooling and must not exist (CI: `pnpm check:node-modules` →
`audit-nested-node-modules.mjs --strict`):

| Path | Reason |
|------|--------|
| `apps/node_modules/` | Stray install (e.g. Prisma output in wrong place) |
| `apps/*/node_modules/` | App-level install outside root |
| `apps/arquetipos/frontend/*/node_modules/` | Frontend app local install |
| `libs/base/backend/prisma/{multi,single}/generated/node_modules/` | Generated Prisma client pollution |

**Allowed:** `libs/**/node_modules` symlinks from pnpm `node-linker=isolated` (not real installs).

**Also never commit:** `**/dist/`, `**/coverage/` (see `.gitignore`). Local folders may appear after build/test — delete them; do not `git add -f`.

### Cleanup (F36-P0-A)

Prefer the scripts — **do not** run `git clean -fdx` (wipes local env and IDE state).

**Bash:**

```bash
node tools/dx/clean-nested-node-modules.mjs
# optional local artifacts (untracked):
rm -rf libs/base/backend/coverage apps/**/dist
pnpm install
pnpm check:node-modules
```

**PowerShell:**

```powershell
node tools/dx/clean-nested-node-modules.mjs
Remove-Item -Recurse -Force -ErrorAction SilentlyContinue libs/base/backend/coverage
Get-ChildItem -Path apps -Recurse -Directory -Filter dist -ErrorAction SilentlyContinue |
  Remove-Item -Recurse -Force
pnpm install
pnpm check:node-modules
```

`pnpm prisma:generate` and root `postinstall` both run the clean script so Prisma
does not leave forbidden `generated/node_modules`.

**Workspace exclude:** `pnpm-workspace.yaml` ignores `libs/**/prisma/**/generated`
so the generated client `package.json` is not treated as a workspace package
(which would recreate `generated/node_modules` on every `pnpm install`).

## Workspace protocol

Internal imports must be declared in the consumer `package.json`:

```json
"@josanz/clients-data-access": "workspace:*"
```

Audit:

```bash
pnpm check:workspace-deps        # report
pnpm check:workspace-deps:strict # fail on missing declarations
```

Si `strict` falla: declara la dep con `pnpm add <pkg> --filter <consumer> --workspace`
(plan [F52-A2](../plans/rounds/plans-52-fifty-two-round/1750000071000-f52-fix-undeclared-workspace-deps.md)).
No uses versión npm fija (`"0.0.0"`) para paquetes privados del monorepo.

## Nx daemon

See **[nx-daemon.md](./nx-daemon.md)** (F36-P0-B) for diagnosis, timings, and the
official workaround.

**Linux / macOS:** daemon activo por defecto; es estable y más rápido en caliente
(~3s vs ~10s sin daemon).

**Windows:** `NX_DAEMON=false` por defecto. El daemon se cuelga con frecuencia
en este workspace por escala del grafo y fan-out de plugins; el costo supera
al beneficio.

```powershell
$env:NX_DAEMON = 'false'
pnpm exec nx show projects   # target: < 30s
```

**CI:** Linux con `NX_DAEMON=false` como workaround histórico; reversible cuando
se estabilice.

Warm daemon es más rápido cuando está saludable, pero cold starts tras `nx reset`
y presión de plugin-workers en Windows suelen parecer hangs (>60s). Preferir
deshabilitar el daemon en Windows antes que esperar indefinidamente.

If Nx Console still shows disabled after removing a forced `NX_DAEMON=false`:

```bash
pnpm nx:daemon:start   # or: pnpm exec nx daemon --start
pnpm exec nx daemon    # should report “currently running”
```

If the daemon hangs, use `pnpm typecheck:affected` and `npx tsc` (see `docs/README.md` §3).
Run `pnpm exec nx reset` only when no other Nx/IDE process is using `.nx/workspace-data`
(close Nx Console tasks first — `EPERM` on Windows if the folder is locked).

### Plugin worker timeouts (Windows)

Symptom: every `nx serve` / `nx show` fails with
`Plugin Worker … did not receive a load message within 10 seconds` and
`Failed to load @nx/eslint/plugin | project-json | jest | …`.

Usual cause: **too many parallel `nx run *:serve` / hung workers**, not a bad app.
The isolated plugin pool cannot handshake in time under process pressure.

Recovery:

1. Stop excess serves (close extra terminals). Do **not** kill the IDE wholesale —
   target `nx run`, `plugin-worker`, daemon, and app CLIs (`ng` / `next` / `expo`).
2. `pnpm nx daemon --stop` then `pnpm nx reset`
3. Confirm: `pnpm nx show projects` (should list projects; first cold graph can take
   1–2 min on a large Windows workspace)
4. If workers still time out: `$env:NX_ISOLATE_PLUGINS='false'` (in-process plugins)
   or `$env:NX_PLUGIN_NO_TIMEOUTS='true'`, then retry `pnpm nx show projects`

Avoid launching many `serve` targets at once on Windows.
