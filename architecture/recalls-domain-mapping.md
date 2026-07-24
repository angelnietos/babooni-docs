<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Recalls — mapeo de dominio (`@ideauto/*`)</h1>

<p align="center">
  <b>Legacy → cliente Ideauto</b>
</p>

<p align="center">
  <img alt="architecture" src="https://img.shields.io/badge/architecture-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../ideauto/migration/"><img alt="F84" src="https://img.shields.io/badge/plan-F84-14b8a6?style=flat-square" /></a>
</p>

Cuándo usarla: crear libs Nx Ideauto, nombrar módulos Nest, decidir `@base` vs `@ideauto`.

---

## Colocación

| Tipo | Dónde |
|------|-------|
| Composition roots | `apps/clientes/ideauto/recalls/{backend,frontend}` |
| Dominio producto | `libs/clientes/ideauto/` → npm **`@ideauto/…`** |
| Kernel | `@base/*` |
| Prohibido | `@arquetipos/*`, `@saas/*`, `@josanz/*` |

Ideauto es **producto cliente** (como Josanz), no SaaS.  
Checklist: [nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md).

Frontend: **Next.js** (opt-in [ADR 0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)); capas de dominio según contrato FE.

---

## Mapping

| Legacy | Destino | Paquetes |
|--------|---------|----------|
| Users, login, profiles | `@base` | `@base/users-*` |
| Clients, ContactPersons | `@base` (+ extensión) | `@base/clients-*` |
| Campaigns, vehicles, typologies, history | `@ideauto` | `@ideauto/campaigns-*` |
| Waves, Letters | `@ideauto` | campaigns subdominio o `@ideauto/waves-*` |
| Budgets | `@ideauto` | `@ideauto/budgets-*` |
| Invoices, Certificates | `@ideauto` | `@ideauto/invoices-*` (≠ Verifactu) |
| Dgt* | `@ideauto` | `@ideauto/dgt-*` |
| FileStructures, filesApi | `@ideauto` | `@ideauto/files-*` |
| Notes | `@ideauto` | notes o feature campaigns |
| Reports / renting | `@ideauto` | `@ideauto/reports-*` |
| admin / dashboard | `@ideauto` | admin shell + features |
| tasks | worker | bajo `clientes/ideauto` |

---

## Árbol objetivo

```
libs/clientes/ideauto/
├── shared/                 # @ideauto/shared          (M0 ✓)
├── backend/                # @ideauto/backend         (M0 ✓)
├── angular-ui/             # @ideauto/angular-ui      (M0 ✓)
├── angular/platform/       # @ideauto/platform-*      (M0 ✓)
└── frontend/next/          # dominios Next (M1+)
    └── {campaigns,dgt,…}/{api,data-access,shell,features}

apps/clientes/ideauto/recalls/
├── backend/                # ideauto-recalls-api (M0 stub)
└── frontend/               # ideauto-recalls-web (M0 stub)
```

---

## Enlaces

- [recalls-migration-strategy.md](./recalls-migration-strategy.md)
- [F84-C1](../ideauto/recalls/domain-map.md)
- [add-frontend-domain.md](../guides/add-frontend-domain.md)
- [add-backend-domain.md](../guides/add-backend-domain.md)
