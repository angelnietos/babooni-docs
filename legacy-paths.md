<p align="center">
  <img src="./assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Legacy path mapping</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Historical docs and early plans refer to folders that were **renamed**. Use this
table when reading first-round plans or old branches.

## Libs (F5–F8)

| Legacy path / name | Current location | Notes |
|--------------------|------------------|-------|
| `libs/node/` | `libs/base/backend/` | Nest hex kernel + Prisma |
| `libs/isomorphic/` | `libs/base/shared/` | DTOs and contracts (`@base/shared`) |
| `libs/browser/angular/` | `libs/base/frontend/angular/` | UI + features + data-access |
| `libs/browser/react/` | `libs/base/frontend/react/` | React stack |
| `@arquetipos/browser-angular` | `@arquetipos/angular` | Package rename F8-DX4 |
| `libs/productos-saas/crm/browser/` | `libs/productos-saas/crm/frontend/angular/` | F6-S1 |
| `libs/productos-saas/crm/isomorphic/` | `libs/productos-saas/crm/shared/` | F6-S1 |
| `libs/productos-saas/crm/node/` | `libs/productos-saas/crm/backend/` | F6-S1 |

CI: `node tools/checks/check-legacy-paths.mjs` (`pnpm check:legacy-paths`).

## Tools (post-F56)

| Legacy path | Current location | Notes |
|-------------|------------------|-------|
| `tools/scripts/<file>.mjs` | `tools/{checks,dx,jest,scaffolds,seeds,smoke,migrate,publish,typecheck}/` | Agrupado por utilidad |
| `tools/scripts/lib/` | `tools/lib/` | Helpers compartidos |
| `tools/ai-migrations/` | `tools/ai/migrations/` | Notas Nx AI |

Mapa vivo: [runbooks/tools-layout.md](./runbooks/tools-layout.md) · [`tools/README.md`](../tools/README.md).

See also [docs/README.md](./README.md), [architecture/overview.md](./architecture/overview.md) and [AGENTS.md](../AGENTS.md).
