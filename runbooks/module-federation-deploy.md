# Module Federation — shared contracts & partial deploy (F38-H8)

## Inventory (Arquetipos MF)

| App (Nx) | Path | Role | Port |
|----------|------|------|------|
| `mf-host-angular` | `apps/arquetipos/frontend/angular/microfrontend/mf-host` | Angular host (runtime `init`) | 4210 |
| `mf-remote-react` | `…/react/microfrontend/mf-remote-clients` | React clients remote | 4211 |
| `mf-remote-audit` | `…/react/microfrontend/mf-remote-audit` | React audit remote | 4212 |

## Required `shared` singletons

Host (`app.config.ts` → `init({ shared })`) and both remotes (`vite.config.mts` → `federation({ shared })`) must agree on:

| Package | Why singleton |
|---------|----------------|
| `react` / `react-dom` | One React tree |
| `@reduxjs/toolkit` / `react-redux` | One Redux runtime |
| `@base/shared-store` | One cross-framework store (`globalThis` Symbol) |
| `@base/shared` | One DTO/contract copy (F38-H8) |

Host marks these **eager**; remotes mark **singleton: true** (not eager) so they consume the host share scope.

## Gate script

```bash
node tools/scripts/check-mf-shared.mjs
```

Fails if a remote omits a required singleton key or if the host inventory drifts.

## Partial-deploy checklist

1. **Never** deploy only a remote that bumps `@base/shared` / `@base/shared-store` without the host (or vice versa) unless versions remain binary-compatible.
2. Deploy order: **host first** (owns eager singletons) → remotes. Rolling remotes alone is OK when shared contracts did not change.
3. After deploy, smoke: open `http://<host>/react/clients` and `/react/audit` — no RUNTIME-015 / duplicate React warnings in console.
4. If DTOs change (`ClientDto`, auth claims), bump host + all remotes in the **same** release train.
5. Local smoke: `pnpm arq:fe:mf` then hit `/dashboard` (shared-store counters) + one React remote route.

## Smoke (no dedicated e2e yet)

```bash
node tools/scripts/check-mf-shared.mjs
pnpm nx show project mf-host-angular --json   # targets: build/serve exist
```

Full UI smoke remains manual / future Playwright against `mf-host-angular`.
