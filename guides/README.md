# Guías prácticas — recetas por tarea

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
| [sonar-local.md](./sonar-local.md) | SonarQube local |
| [pr-checklist.md](./pr-checklist.md) | Checklist antes de abrir PR |
| [ui-re-export-vs-wrapper.md](./ui-re-export-vs-wrapper.md) | Re-export vs wrapper en UI de producto |

## Checklists existentes (detalle)

- [clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md) — scaffold completo `@acme/*`
- [frontend/arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md) — plantillas sin duplicar base
- [frontend/josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md) — audit, users, angular-ui
- [frontend/design-system.md](../frontend/design-system.md) — tokens, Storybook, catálogo
- [backend/backend-domain-convention.md](../backend/backend-domain-convention.md) — slugs, BD, empaquetados
- [productos-saas/productos-saas-extends-base.md](../productos-saas/productos-saas-extends-base.md) — SaaS sobre base

## Generadores y scripts

| Script | Uso |
|--------|-----|
| `tools/scripts/new-domain.mjs` | Dominio hexagonal en `@base/backend` (`--dry` preview) |
| `tools/scripts/scaffold-josanz-domain.mjs` | 4 capas frontend + backend Josanz |
| `tools/scripts/scaffold-cliente-product.mjs` | Esqueleto `@acme/shared`, backend, platform |
| `tools/scripts/check-domain-conventions.mjs` | Valida layout hex backend |
| `tools/scripts/check-frontend-conventions.mjs` | Valida layout/pages/components |
| `tools/scripts/check-lib-layout.mjs` | Path físico vs categoría lib |
| `tools/scripts/check-ui-ownership.mjs` | UI base vs josanz vs arquetipos |

## Verificación mínima tras cualquier guía

```bash
pnpm nx typecheck <project>
node tools/scripts/check-lib-layout.mjs
node tools/scripts/check-frontend-conventions.mjs   # si tocaste frontend
```
