# Legacy path mapping (F8-DX4)

Historical docs and early plans refer to folders that were **renamed during F5–F7**.
Use this table when reading first-round plans or old branches.

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

CI enforces no live references via `node tools/scripts/check-legacy-paths.mjs`.

See also [docs/README.md](./README.md), [architecture/overview.md](./architecture/overview.md) and [AGENTS.md](../AGENTS.md).
