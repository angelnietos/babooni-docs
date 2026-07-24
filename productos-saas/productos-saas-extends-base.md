<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Productos SaaS — extiende `@base/*`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

**ID plan:** F7-A2 (complemento arquitectura)  
**Ámbito:** `libs/productos-saas/`, `apps/productos-saas/`

Los productos SaaS (`@saas/*`) son **capa de producto independiente** del ERP Josanz, con la misma regla que clientes y arquetipos: **construir sobre `@base/*`**, no reimplementar kernel.

---

## Regla de dependencias

```
@base/*  ←  @saas/*  ←  apps/productos-saas/*
         ↑
    @josanz/*   (solo donde el producto reutiliza dominio ERP — p. ej. document-generator)
```

| Origen | Puede importar | No puede importar |
|--------|----------------|-------------------|
| `layer:productos-saas` | `layer:base`, `layer:productos-saas`, `layer:clientes` | `layer:arquetipos` |
| Apps `apps/productos-saas/` | `@saas/*`, `@base/*`, `@josanz/*` (casos documentados) | `@arquetipos/*` |

ESLint: `eslint.config.mjs` → `layer:productos-saas`.

---

## Extensión por capa

| Capa SaaS | Patrón «extiende base» | Ejemplo |
|-----------|------------------------|---------|
| **Modelo isomórfico** | `@saas/shared-model` re-exporta DTOs kernel + tipos CRM propios | `ClientDto` desde `@base/shared` |
| **UI Angular** | `@saas/shared-ui` re-exporta `@base/angular-ui` + componentes marca CRM (`gcrm-*`) | Igual que `@josanz/angular-ui` → base |
| **HTTP / data-access** | Servicios dominio usan helpers CRM; **alinear** con `@base/angular-api` (`ApiClient`, interceptors) | `@saas/shared-browser-data-access` |
| **Features** | Componen `@saas/shared-ui` (que incluye base); no HTML crudo | `@saas/invoices-feature` |
| **Backend CRM** | Nest por dominio en `crm/backend/{domain}/`; kernel auth/tenant vía `@saas/shared-infrastructure` | No duplicar hex de `@base/backend` salvo integración ERP futura |
| **Verifactu ledger** | Dominio fiscal propio (`verifactu/core`, `adapters`) — **no** hay equivalente en base | Excepción producto |

---

## Árbol canónico (objetivo)

```
libs/productos-saas/
├── crm/
│   ├── shared/                    # @saas/shared-model — extiende @base/shared
│   ├── backend/{domain}/          # @saas/{domain}-backend
│   └── frontend/angular/
│       ├── shared/
│       │   ├── data-access/       # @saas/shared-browser-data-access — kernel HTTP CRM
│       │   └── ui/                # @saas/shared-ui — extiende @base/angular-ui
│       └── {domain}/              # api, data-access, shell, features
│           ├── clients/
│           ├── identity/
│           ├── invoices/          # frontend slug; backend → invoicing
│           └── verifactu/
└── verifactu/
    ├── adapters/                  # worker BullMQ
    ├── core/                      # ledger ERP
    └── crm-core/                  # cola UI CRM
```

**Legacy en migración:** `crm/browser/`, `crm/isomorphic/`, `crm/node/` — ver plan F6-S1.

---

## Apps → lazy shells

| App | Ruta app | Lazy shell / lib |
|-----|----------|------------------|
| **verifactu-platform** | `/identity` | `@saas/identity-shell` |
| | `/clients` | `@saas/clients-shell` |
| | `/invoices` | `@saas/invoices-shell` |
| | `/verifactu` | `@saas/verifactu-shell` |
| **recalls** (F84) | TBD por dominio | `@saas/campaigns-shell`, `@saas/dgt-shell`, … — ver [recalls-domain-mapping.md](../architecture/recalls-domain-mapping.md) |
| **document-generator** | `/documents` | `@josanz/documents-shell` (dominio ERP reutilizado) |
| | auth | `@base/angular` (`authGuard`) + rutas app |
| **verifactu-crm-api** | REST | `@saas/clients`, `@saas/identity`, `@saas/invoicing-backend`, `@saas/verifactu` |
| **verifactu-worker** | BullMQ | `@saas/verifactu-adapters` |

Detalle operativo: [apps/productos-saas/README.md](../../apps/productos-saas/README.md).

---

## Comparativa con otros scopes

| Scope | Relación con `@base/*` | UI primaria |
|-------|------------------------|-------------|
| **Arquetipos** | Thin shell+features → base | `@arquetipos/*-ui` o `@base/*-ui` |
| **Josanz** | Backend hex + dominios producto; frontend 4 capas | `@josanz/angular-ui` → base |
| **SaaS** | Modelo + UI + HTTP kernel extienden base; dominios fiscales propios | `@saas/shared-ui` → base |

---

## Checklist — nuevo dominio `@saas/{domain}-*`

1. Tipos: extender `@base/shared` en `@saas/shared-model` o `{domain}-api` si aplica.
2. `crm/frontend/angular/{domain}/{api,data-access,shell,features}/`
3. Backend: `crm/backend/{domain}/` si hay API Nest propia.
4. Features: importar UI solo desde `@saas/shared-ui` (nunca `@base/angular-ui` directo en features).
5. Paths en `tsconfig.base.json`; tags `layer:productos-saas`, `runtime:angular|backend|isomorphic`.
6. Entrada en `verifactu-platform` `app.routes.ts` si es ruta de producto.

---

## Deuda conocida (no bloquea F7-A2)

| Área | Estado | Objetivo |
|------|--------|----------|
| `shared-browser-data-access` | HttpClient + interceptors CRM | Envolver `@base/angular-api` / `NgApiClient` |
| `clients` / `identity` backend | Prisma CRM aislado | Mantener; puente ERP vía webhooks/env |
| Legacy `crm/browser/` | Duplicado paths | Consolidar en `crm/frontend/angular/` (F6-S1) |
| Promoción `gcrm-*` → base | Componentes genéricos en CRM UI | F7-UI* promotionCandidates |

---

## Verificación

```bash
node tools/checks/check-lib-layout.mjs --strict
node tools/checks/check-ui-ownership.mjs
npx tsc -p apps/productos-saas/verifactu-platform/tsconfig.app.json --noEmit
npx tsc -p libs/productos-saas/crm/frontend/angular/shared/ui/tsconfig.lib.json --noEmit
```

---

## Enlaces

- [AGENTS.md](../../AGENTS.md)
- [backend-domain-convention.md](../backend/backend-domain-convention.md) — slug `invoices` ↔ `invoicing`
- [ui-component-catalog.yaml](../frontend/ui-component-catalog.yaml)
- [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md)
