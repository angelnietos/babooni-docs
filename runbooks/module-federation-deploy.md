<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Module Federation — shared contracts & partial deploy (F38-H8)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

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
node tools/checks/check-mf-shared.mjs
```

Fails if a remote omits a required singleton key or if the host inventory drifts.

## Partial-deploy checklist

1. **Never** deploy only a remote that bumps `@base/shared` / `@base/shared-store` without the host (or vice versa) unless versions remain binary-compatible.
2. Deploy order: **host first** (owns eager singletons) → remotes. Rolling remotes alone is OK when shared contracts did not change.
3. After deploy, smoke: open `http://<host>/react/clients` and `/react/audit` — no RUNTIME-015 / duplicate React warnings in console.
4. If DTOs change (`ClientDto`, auth claims), bump host + all remotes in the **same** release train.
5. Local smoke: `pnpm arq:fe:mf` then hit `/dashboard` (shared-store counters) + one React remote route.

## Smoke e2e (F58-D2)

```bash
node tools/checks/check-mf-shared.mjs
pnpm arq:fe:mf                                          # host :4210 + remotes :4211/:4212
pnpm nx run mf-host-e2e:e2e -- --project=chromium       # soft in CI e2e-web
# or full arquetipos set:
pnpm arq:fe:e2e:smoke
```

Playwright config starts host + remotes (`mf-remote-react`, `mf-remote-audit`,
`mf-host-angular`). Ver matriz en [parity.md](../arquetipos/parity.md).
