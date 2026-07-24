<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="48" alt="Arquetipos" />
</p>

<h1 align="center">Mapa de dominio → `@ideauto/*`</h1>

<p align="center">
  <a href="./README.md"><img alt="Recalls" src="https://img.shields.io/badge/hub-Recalls-0f766e?style=flat-square" /></a>
</p>

---

## Principio

| Tipo | Destino |
|------|---------|
| Kernel genérico | `@base/*` |
| Negocio Recalls Ideauto | `@ideauto/{domain}-*` |
| Nunca | `@saas/*`, `@josanz/*`, `@arquetipos/*` |

---

## Tabla

| Legacy | Destino | Paquetes |
|--------|---------|----------|
| Users, login, password, profiles | `@base` + wiring | `@base/users-*`, guards Nest |
| Clients, ContactPersons | `@base` (+ extensión) | `@base/clients-*` |
| Campaigns, vehicles, typologies, history | `@ideauto` | `@ideauto/campaigns-*` |
| Waves, Letters | `@ideauto` | subdominio campaigns o `@ideauto/waves-*` |
| Budgets | `@ideauto` | `@ideauto/budgets-*` |
| Invoices, Certificates | `@ideauto` | `@ideauto/invoices-*` (≠ Verifactu) |
| Dgt* | `@ideauto` | `@ideauto/dgt-*` (SOAP solo aquí) |
| FileStructures, filesApi | `@ideauto` | `@ideauto/files-*` |
| Notes | `@ideauto` | notes o feature campaigns |
| Reports / renting | `@ideauto` | `@ideauto/reports-*` |
| admin / dashboard | `@ideauto` | admin shell + features |
| tasks (node-schedule) | worker | bajo `clientes/ideauto` |

### Modelos Sequelize (24)

`Budgets`, `Campaigns`, `CampaignsHistory`, `CampaignTypologies`, `CampaignVehicles`, `Certificates`, `Clients`, `ContactPersons`, `DgtAddresses`, `DgtDeregistrations`, `DgtNotFound`, `DgtRecall`, `DgtRecallNotification`, `DgtReferences`, `FileStructures`, `IdeautoReferences`, `Invoices`, `Letters`, `Notes`, `RentingCompanies`, `Users`, `VinsInactiveNonCampaign`, `Waves`.

Ninguno huérfano.

---

## Paquetes mínimos

```
@ideauto/shared
@ideauto/backend
@ideauto/campaigns-{api,data-access,shell,features}
@ideauto/dgt-{api,data-access,shell,features}
@ideauto/budgets-* · @ideauto/files-* · @ideauto/reports-*
apps/clientes/ideauto/recalls/{backend,frontend}
```

---

## Enlaces

- [scope-and-placement.md](./scope-and-placement.md)  
- [F84-C1](../../plans/rounds/plans-84-eighty-four-round/1764000022000-f84-domain-mapping.md)  
- Canónico architecture: [recalls-domain-mapping.md](../../architecture/recalls-domain-mapping.md)
