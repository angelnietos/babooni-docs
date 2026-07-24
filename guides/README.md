<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Guías prácticas</h1>

<p align="center">
  Recetas por tarea · <b>qué hacer</b> · <b>dónde ponerlo</b> · <b>por qué</b>
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../architecture/overview.md"><img alt="Overview" src="https://img.shields.io/badge/contexto-overview-14b8a6?style=flat-square" /></a>
  <a href="../CONTRIBUTING-DOCS.md"><img alt="Style" src="https://img.shields.io/badge/style-CONTRIBUTING--DOCS-0d5f59?style=flat-square" /></a>
</p>

Cada guía responde: **qué hacer**, **dónde ponerlo** y **por qué**. Lee [architecture/overview.md](../architecture/overview.md) si necesitas contexto previo.

Estilo de autoría: [CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md).

## Dominio y producto

| Guía | Cuándo usarla |
|------|---------------|
| [local-development.md](./local-development.md) | Infra, seeds, puertos, servir apps, depurar env |
| [add-frontend-domain.md](./add-frontend-domain.md) | Nueva pantalla / dominio Angular o React |
| [add-backend-domain.md](./add-backend-domain.md) | Nuevo endpoint / módulo Nest |
| [add-microservice.md](./add-microservice.md) | Extraer dominio a servicio gRPC/HTTP propio |
| [new-client-product.md](./new-client-product.md) | Nuevo producto cliente (tipo Josanz) |
| [new-product-e2e-walkthrough.md](./new-product-e2e-walkthrough.md) | Narrativa única: scaffold → dominio → BD → auth → CI |
| [extend-kernel-domain.md](./extend-kernel-domain.md) | Extender dominio `@base/*` sin producto |
| [scaffold-arquetipos.md](./scaffold-arquetipos.md) | Scaffold plantillas Arquetipos |
| [add-mobile-domain.md](./add-mobile-domain.md) | Ionic + React Native (single/multi) |
| [add-next-domain.md](./add-next-domain.md) | Dominio Next.js thin vs SPA |
| [module-federation-dev.md](./module-federation-dev.md) | MF local (host + remotes) |

## Backend, auth y calidad

| Guía | Cuándo usarla |
|------|---------------|
| [testing-pyramid.md](./testing-pyramid.md) | Unit / int / e2e, harnesses, Playwright |
| [backend-use-case-tests.md](./backend-use-case-tests.md) | Specs de handlers CQRS (detalle) |
| [backend-uow-jobs-messaging.md](./backend-uow-jobs-messaging.md) | UoW/ALS en BullMQ + messaging |
| [backend-jobs-shutdown.md](./backend-jobs-shutdown.md) | SIGTERM / BullMQ lockDuration |
| [ai-cqrs-policy.md](./ai-cqrs-policy.md) | Boundary AI sobre CommandBus/QueryBus |
| [auth-jwks-fail-closed.md](./auth-jwks-fail-closed.md) | JWKS / Keycloak fail-closed |
| [keycloak-setup.md](./keycloak-setup.md) | Realms, clients, FE OIDC |
| [roles-rbac.md](./roles-rbac.md) | Roles DB + Keycloak + fieldPolicies |
| [api-versioning.md](./api-versioning.md) | Versionado de API HTTP |
| [deprecation-policy.md](./deprecation-policy.md) | Deprecaciones y `check:deprecated` |
| [npm-publish-and-versioning.md](./npm-publish-and-versioning.md) | Publicar libs + semver (F51-E1 + F52-A1 workflow) |
| [sonar-local.md](./sonar-local.md) | SonarQube local |
| [pr-checklist.md](./pr-checklist.md) | Checklist antes de abrir PR |
| [ui-re-export-vs-wrapper.md](./ui-re-export-vs-wrapper.md) | Re-export vs wrapper en UI de producto |

## Checklists existentes (detalle)

- [clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md) — scaffold completo `@acme/*`
- [frontend/arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md) — plantillas sin duplicar base
- [frontend/josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md) — audit, users, angular-ui
- [frontend/design-system.md](../frontend/design-system.md) — tokens, Storybook, catálogo
- [frontend/ui-strategy.md](../frontend/ui-strategy.md) — Lit SoT + freeze ([ADR 0010](../adr/adr-0010-native-ui-lit-sot.md))
- [adr/README.md](../adr/README.md) — decisiones (incl. 0010/0011 UI)
- [backend/backend-domain-convention.md](../backend/backend-domain-convention.md) — slugs, BD, empaquetados
- [productos-saas/productos-saas-extends-base.md](../productos-saas/productos-saas-extends-base.md) — SaaS sobre base
- [architecture/recalls-v2-assessment.md](../architecture/recalls-v2-assessment.md) — assessment técnico
- [ideauto/recalls/](../ideauto/recalls/) — **doc migración Ideauto** (producto/PM/devs)
- [runbooks/recalls-migration.md](../runbooks/recalls-migration.md) — milestones M0–M6 (`@ideauto/*`)

## Generadores y scripts

Mapa completo: [runbooks/tools-layout.md](../runbooks/tools-layout.md). Prefer `pnpm <script>`.

| Script / comando | Uso |
|------------------|-----|
| `tools/scaffolds/new-domain.mjs` | Dominio hexagonal en `@base/backend` (`--dry` preview) |
| `tools/scaffolds/scaffold-josanz-domain.mjs` | 4 capas frontend + backend Josanz |
| `tools/scaffolds/scaffold-cliente-product.mjs` | Esqueleto `@acme/shared`, backend, platform |
| `pnpm check:domain-conventions` | Valida layout hex backend |
| `pnpm check:frontend-conventions` | Valida layout/pages/components |
| `pnpm check:lib-layout` | Path físico vs categoría lib |
| `pnpm check:ui-ownership` | UI base vs josanz vs arquetipos |
| `pnpm check:workspace-deps:strict` | Imports `@scope/*` sin `workspace:*` |
| `pnpm check:publishable-deps` | Libs `tag:publishable` sin deps workspace private |
| `pnpm pack:publishable` | `npm pack` canario + reescritura `workspace:*` |
| `pnpm mockserver` | FE plantilla sin Nest ([mockserver](../runbooks/mockserver.md)) |

## Verificación mínima tras cualquier guía

```bash
pnpm nx typecheck <project>
pnpm check:lib-layout
pnpm check:frontend-conventions   # si tocaste frontend
pnpm check:workspace-deps:strict  # si tocaste package.json / imports entre libs
```
