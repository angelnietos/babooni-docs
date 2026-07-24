# F86-F — Platform config server (FE + BE)

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Disponer de un **config server de plataforma** que centralice configuración
runtime consumible por **backend y frontend** (feature flags, branding tenant,
endpoints, límites, temas, toggles de integración), sin que cada app reinvente
`.env` ad-hoc ni duplique settings.

## Contexto

Hoy existe dominio `settings` (CRUD tenant-scoped en `@base/backend` + shells FE).
Eso cubre preferencias de producto, **no** un contrato unificado de configuración
de plataforma multi-app / multi-runtime.

Clientes corporativos necesitan: cambiar config sin redeploy, mismos valores
visibles en Nest y en Angular/React/Next, y overrides por tenant/entorno.

## Alcance

### In

- Contrato isomórfico de config (`@base/shared` o paquete dedicado): claves tipadas,
  scopes (`platform` | `tenant` | `app`), sensibilidad (public vs secret).
- **Backend:** módulo Nest “config server” (ports + adapters) que resuelve
  cascada: defaults código → env → store DB/settings → provider externo (Azure App
  Configuration / Key Vault — ver F86-G).
- **Frontend:** cliente de config en `*-api` / kernel (`@base/angular-api` /
  `@base/react-api`) + bootstrap en shell (cargar config pública antes de routes).
- Endpoints públicos vs autenticados (solo exponer claves `public`).
- Cache + invalidación (TTL / ETag / version stamp).
- Extender o componer el dominio `settings` existente — no duplicar CRUD sin
  necesidad.

### Out (por ahora)

- UI admin completa de feature flags (puede ser MVP read/write vía settings).
- Sustituir secrets de CI/CD (sigue `.env` / vault en deploy).

## Acciones

- [ ] ADR corto: config server vs settings domain vs env-only
- [ ] Modelo de claves + schema Zod isomórfico
- [ ] Port `ConfigPort` + adapter DB (settings) + adapter env
- [ ] HTTP: `GET /api/platform-config` (public) + admin mutate (RBAC)
- [ ] Cliente FE + hook/facade de bootstrap
- [ ] Docs: runbook + enlace en [platform-as-product.md](../../../architecture/platform-as-product.md)
- [ ] Smoke: BE sirve config; FE lee y aplica al menos 1 flag / 1 branding key

## Criterios

- [ ] Una app plantilla y un producto cliente consumen la **misma** API de config
- [ ] Secretos nunca llegan al bundle FE
- [ ] typecheck + tests del módulo verdes; gate layout respetado

## Relacionado

- Dominio actual: `libs/base/backend/src/lib/domains/settings/`
- Microsoft providers: [F86-G](1766000007000-f86-microsoft-azure-data-plane.md)
- Reliability: [F86-C](1766000003000-f86-kernel-reliability.md)
