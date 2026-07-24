<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Recalls — mapeo de dominio</h1>

<p align="center">
  <b>Legacy entities → paquetes monorepo</b>
</p>

<p align="center">
  <img alt="architecture" src="https://img.shields.io/badge/architecture-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-84-eighty-four-round/"><img alt="F84" src="https://img.shields.io/badge/plan-F84-14b8a6?style=flat-square" /></a>
</p>

Cuándo usarla: al crear libs Nx, nombrar módulos Nest, o decidir si algo es `@base` o `@saas`.

---

## Colocación

| Tipo | Dónde |
|------|-------|
| Composition roots | `apps/productos-saas/recalls/{backend,frontend}` |
| Dominio producto | `libs/productos-saas/recalls/` → npm `@saas/…` |
| Kernel reutilizado | `@base/*` (users, clients, audit, prisma, api client) |
| Prohibido | Importar `@arquetipos/*` desde Recalls |

Recalls es **producto SaaS** independiente del ERP Josanz: misma regla que Verifactu ([productos-saas-extends-base.md](../productos-saas/productos-saas-extends-base.md)).

Frontend: **Next.js** (opt-in [ADR 0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)) porque el legacy ya es Next y el equipo conoce ese runtime; las **capas de dominio** siguen el contrato React/Next del monorepo.

---

## Mapping

| Legacy | Destino | Paquetes |
|--------|---------|----------|
| Users, login, password, profiles | `@base` + wiring producto | `@base/users-*`, guards Nest |
| Clients, ContactPersons | `@base` (+ extensión saas) | `@base/clients-*` |
| Campaigns, CampaignVehicles, Typologies, History | `@saas` | `@saas/campaigns-{api,data-access,shell,features}` + BE |
| Waves, Letters | `@saas` | Subdominio campaigns **o** `@saas/waves-*` |
| Budgets | `@saas` | `@saas/budgets-*` |
| Invoices, Certificates | `@saas` | `@saas/invoices-*` / certificados producto (≠ Verifactu fiscal) |
| Dgt* (recall, addresses, deregistrations, …) | `@saas` | `@saas/dgt-*` (SOAP solo aquí) |
| FileStructures, filesApi | `@saas` | `@saas/files-*` |
| Notes | `@saas` | notes lib o feature campaigns |
| RentingCompanies, reports | `@saas` | `@saas/reports-*` |
| IdeautoReferences, VinsInactive*, vehicleInfo | `@saas` | campaigns / vehicles |
| adminHome, dashboard | `@saas` | admin shell + features |
| tasks (node-schedule) | worker | app worker o task runner base |

### Modelos Sequelize (inventario)

`Budgets`, `Campaigns`, `CampaignsHistory`, `CampaignTypologies`, `CampaignVehicles`, `Certificates`, `Clients`, `ContactPersons`, `DgtAddresses`, `DgtDeregistrations`, `DgtNotFound`, `DgtRecall`, `DgtRecallNotification`, `DgtReferences`, `FileStructures`, `IdeautoReferences`, `Invoices`, `Letters`, `Notes`, `RentingCompanies`, `Users`, `VinsInactiveNonCampaign`, `Waves`.

---

## Qué no se migra “tal cual”

- Atomic design folders como arquitectura de producto.
- `queryBuilder.js` y repos duplicados.
- Endpoints públicos de usuarios.
- Redux slices de UI global como fuente de verdad de dominio.
- Lógica DGT repartida en utils/services sueltos.

---

## Árbol objetivo (resumen)

```
libs/productos-saas/recalls/
├── shared/                 # @saas/recalls-shared
├── backend/                # módulos Nest por dominio
└── frontend/next/
    └── {campaigns,dgt,…}/{api,data-access,shell,features}
```

Contrato: [frontend-domain-architecture-contract.md](../frontend/frontend-domain-architecture-contract.md).

---

## Enlaces

- [recalls-migration-strategy.md](./recalls-migration-strategy.md)
- [F84-C1](../plans/rounds/plans-84-eighty-four-round/1764000022000-f84-domain-mapping.md)
- [add-frontend-domain.md](../guides/add-frontend-domain.md)
- [add-backend-domain.md](../guides/add-backend-domain.md)
