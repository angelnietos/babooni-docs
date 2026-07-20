# Guías prácticas — recetas por tarea

Cada guía responde: **qué hacer**, **dónde ponerlo** y **por qué**. Lee [architecture/overview.md](../architecture/overview.md) si necesitas contexto previo.

| Guía | Cuándo usarla |
|------|---------------|
| [local-development.md](./local-development.md) | Infra, seeds, servir apps, depurar env |
| [add-frontend-domain.md](./add-frontend-domain.md) | Nueva pantalla / dominio Angular o React |
| [add-backend-domain.md](./add-backend-domain.md) | Nuevo endpoint / módulo Nest |
| [backend-use-case-tests.md](./backend-use-case-tests.md) | Specs de handlers / use-cases (F36-P1-B) |
| [backend-uow-jobs-messaging.md](./backend-uow-jobs-messaging.md) | UoW/ALS en BullMQ + messaging (F38-H4) |
| [backend-jobs-shutdown.md](./backend-jobs-shutdown.md) | SIGTERM / BullMQ lockDuration (F38-H11) |
| [auth-jwks-fail-closed.md](./auth-jwks-fail-closed.md) | JWKS / Keycloak fail-closed (F38-H12) |
| [add-microservice.md](./add-microservice.md) | Extraer dominio a servicio gRPC/HTTP propio |
| [new-client-product.md](./new-client-product.md) | Nuevo producto cliente (tipo Josanz) |
| [extend-kernel-domain.md](./extend-kernel-domain.md) | Extender dominio `@base/*` sin producto |
| [roles-rbac.md](./roles-rbac.md) | Roles DB + Keycloak identity + fieldPolicies (Arquetipos) |

## Checklists existentes (detalle)

- [clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md) — scaffold completo `@acme/*`
- [frontend/arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md) — plantillas sin duplicar base
- [frontend/josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md) — audit, users, angular-ui
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
npx tsc -p <lib-o-app>/tsconfig.lib.json --noEmit   # o tsconfig.app.json
node tools/scripts/check-lib-layout.mjs
node tools/scripts/check-frontend-conventions.mjs     # si tocaste frontend
```
