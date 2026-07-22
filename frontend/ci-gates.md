# CI gates (F11 + F12)

Unified hygiene + governance gates enforced in CI (`verify`, `frontend`, and
`quality` jobs in `.github/workflows/ci.yml`). The canonical local entry point
is `pnpm hygiene`, which runs layout, paths, workspace-deps, and slim-barrel
checks in sequence.

| Gate | Script | Fails when |
|------|--------|------------|
| Hygiene | `pnpm hygiene` | layout / paths≠3 / workspace deps / slim barrel |
| Paths | `pnpm check:tsconfig-paths` | paths > 3 or redundant > 0 |
| Workspace | `pnpm check:workspace-deps:strict` | import without `workspace:*` |
| UI | `pnpm check:ui-ownership:strict` | wrong UI layer; @josanz/*-features → `@base/angular-ui` direct |
| Lib layout | `pnpm check:lib-layout:strict` | runtime tags / lib layout violations |
| Legacy paths | `pnpm check:legacy-paths` | legacy `browser/` / `isomorphic/` / `node/` structure |
| Node modules | `pnpm check:node-modules` | nested `node_modules` (stray installs / Prisma copies) |
| Lint coverage | `pnpm check:lint-coverage` | `type:shell\|feature\|ui` project without `lint` target |
| Boundaries smoke | `pnpm lint:boundaries` | module-boundary violations on all shell/feature/ui |
| Slim barrel | `pnpm check:slim-barrel` | domain stores imported from `@base/angular` / `@base/react` in apps |
| Documents parity | `pnpm check:documents-route-parity` | doc-gen routes diverge from josanz-erp (needs `JOSANZ_ERP_ROOT`) |
| Coverage BE strict (F55-C1) | `pnpm test:cov:check` en job `quality` (**fail**) | Below domain threshold (audit/settings/tenants) |
| Jest workspace coverage (F55-C3) | `check:jest-preset` + `test:coverage:affected` + merge → `coverage/global/` | Soft (`continue-on-error`); no umbral workspace fail |
| Arquetipos parity strict (F55-C2) | `pnpm check:arquetipos-parity -- --strict` | Drift rutas/dominios/UI entre stacks plantilla |
| Arquetipos FE build smoke (F56-A1 / F57-C1) | `pnpm arq:fe:build:smoke` (soft CI) | Angular/React single fail to bundle |
| Coverage truth (F57-A1 / **F58-A1** strict) | `check:coverage-truth` (+ `:strict` en F58) | Missing coverage-final / coverage/root dumps |
| MockServer (F56-C1) | `pnpm mockserver` / `mockserver:smoke` | (dev only; not a PR fail gate) |
| Scaffolds smoke (F57-E1) | `pnpm scaffolds:smoke` | CLI/dry-run crash |

Script paths live under `tools/{checks,dx,jest,scaffolds,…}` — see
[runbooks/tools-layout.md](../runbooks/tools-layout.md).

### F12-NM — Base Prisma nested `node_modules` (resolved)

`libs/base/backend/prisma/{multi,single}/generated/` is the committed output of
`prisma generate`. Prisma 7 copies `@prisma/client-runtime-utils` into a nested
`generated/node_modules/` (declared as a `dependency` of the generated client).
That copy is redundant: Node resolves `@prisma/client-runtime-utils@7.8.0` from
the workspace root `node_modules`, so the nested copy is removed harmlessly.

**Fix (Option C + F36-P0-A):** `pnpm prisma:generate` and root `postinstall`
run `clean-nested-node-modules.mjs`. `pnpm-workspace.yaml` excludes
`libs/**/prisma/**/generated` so install does not treat the generated client as
a workspace package (that was recreating `generated/node_modules`). The dirs are
`gitignored`; CI `pnpm check:node-modules` must stay strict.

**`DATABASE_URL`:** `prisma.config.ts` uses a generate-only placeholder when
`DATABASE_URL` is unset, so `pnpm prisma:generate` works on fresh clones and CI
without `.env`. Migrate/deploy and runtime apps still require a real
`DATABASE_URL` (see `.env.example` / `docker-compose.dev.yml`).

Do **not** remove the two entries from `FORBIDDEN_PATTERNS` in
`tools/checks/audit-nested-node-modules.mjs` — the gate must stay strict.

### F12-DX — Typecheck on large graphs

| Scenario | Command |
|----------|---------|
| PR / day-to-day (CI default) | `pnpm typecheck:affected` |
| Touches `tsconfig.base.json`, bulk paths, or root tooling | `pnpm typecheck:all:base` then affected scopes |
| Full layer smoke after structural change | `pnpm typecheck:all:base` + `:arquetipos` + `:saas` + `:josanz` |
| All projects (slow) | `pnpm typecheck:all` |

CI keeps `fetch-depth: 0` on verify so `typecheck:affected` has a correct base.
If `typecheck:affected` times out locally after a large diff vs stale
`origin/main`, refresh the base branch or use scoped `typecheck:all:*` instead.

## Policy

- **tsconfig paths ratchet frozen at 3** (3 Angular testing shims) after
  F11-PRISMA landed `@saas/prisma-client` as a real workspace package.
- New libs must get a `package.json` `name` + `exports` first and resolve via
  pnpm `workspace:*` symlinks — no new tsconfig path.
- Cross-layer UI imports are forbidden by `check:ui-ownership`; product UI must
  come from `@josanz/angular-ui` / `@saas/shared-ui` / `@base/angular-ui`, never
  from another product layer.
- Apps must import `clientsStore|usersStore|auditStore|authStore` from
  `*-data-access` packages, not from `@base/angular` / `@base/react` barrels.

## Local verification

```bash
pnpm hygiene
pnpm check:ui-ownership:strict
pnpm typecheck:affected
pnpm test:affected
# opcional coverage workspace:
# pnpm test:coverage:report:affected
```

Antes de abrir PR: [guides/pr-checklist.md](../guides/pr-checklist.md) (espejo de
`verify` / `frontend` / `quality` en `.github/workflows/ci.yml`). Runbook Jest:
[runbooks/jest-coverage.md](../runbooks/jest-coverage.md).
[typecheck-and-lint-gates.md](../runbooks/typecheck-and-lint-gates.md).

After structural / prisma changes:

```bash
pnpm prisma:generate
pnpm check:node-modules
```

## Audit report

`pnpm audit:tsconfig-paths:report` regenerates
[tsconfig-paths-audit.md](./tsconfig-paths-audit.md).
