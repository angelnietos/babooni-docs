# Dual-stack clients parity (F38-H7)

Canary domain: **clients**. Both stacks consume `ClientDto` / `Paged<ClientDto>`
from `@base/shared`.

| Case | Angular `ClientsService` | React `clientsStore` |
|------|--------------------------|----------------------|
| Empty list | `data: []` → `items()=[]`, `error=null` | `load` fulfilled `[]` → `data=[]` |
| Loading | `_loading` true until settle | `load.pending` → `loading=true` |
| 401 / Unauthorized | clears items; `Sesión expirada o no autorizada.` | clears `data`; error contains `401` |
| Other HTTP fail | clears items; generic ES message | clears `data`; English status message |

**Fixed divergence:** React `createFeatureStore` used to keep stale `data` on
`load.rejected`; Angular always cleared. Both now clear on load failure.

Specs:

```bash
pnpm exec jest -c libs/base/frontend/angular/domains/clients/data-access/jest.config.ts
pnpm exec jest -c libs/base/frontend/react/domains/clients/data-access/jest.config.ts
```

Coordena with F38-COVER C9 (Angular) / C10 (React).
