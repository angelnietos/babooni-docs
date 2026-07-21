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

---

## Ampliación — paridad arquetipos multi-framework (F54-B4)

Este doc cubre **comportamiento** de un dominio canario Angular↔React.

El marco completo exige **tres ejes**:

1. **Arquitectura** — mismas 4 capas / thin libs / apps thin en todos los stacks Full.
2. **Visual** — todos usan `@base/native-ui` (wrappers `Native*` / CE); no UI por framework.
3. **Funcional** — mismas rutas/flujos/contratos (esta tabla clients es el canario).

Plan: [F54-B4](../plans/rounds/plans-54-fifty-four-round/1750000104500-f54-arquetipos-cross-framework-parity.md) ·
Thin libs: [arquetipos-thin-libs.md](./arquetipos-thin-libs.md) ·
SoT UI: [ADR 0010](../adr/adr-0010-native-ui-lit-sot.md).
