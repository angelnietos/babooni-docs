<p align="center">
  <img src="./assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Primer día — arranque local</h1>

<p align="center">
  Guía para un desarrollador nuevo · <b>30–45 min</b> hasta ver Josanz en local
</p>

<p align="center">
  <a href="./README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./architecture/overview.md"><img alt="Overview" src="https://img.shields.io/badge/orientación-overview-14b8a6?style=flat-square" /></a>
</p>

> Si solo quieres orientarte sin levantar nada, lee primero [architecture/overview.md](./architecture/overview.md).

---

## 1. Qué es este repositorio (en una frase)

Un **monorepo Nx** con un kernel reutilizable (`@base/*`), plantillas de referencia (`@arquetipos/*`), productos de cliente (`@josanz/*`, `@ideauto/*`, …) y SaaS (`@saas/*`). Las **apps** componen libs; las **libs** no eligen cómo se despliegan (monolito, microservicio, BD compartida o dedicada).

---

## 2. Prerrequisitos

| Herramienta | Versión orientativa |
|-------------|---------------------|
| Node.js | **20.x o 22.x** (matriz Nx — `pnpm check:node-nx`; evita Node 24 con Nx 23) |
| pnpm | 10+ (`packageManager` en `package.json`) |
| Docker Desktop | Para Postgres, Redis, Kafka, Keycloak |

Sin Docker puedes prototipar UI de plantillas con [MockServer](./runbooks/mockserver.md) (`pnpm mockserver`).

---

## 3. Instalación

```bash
git clone <repo-url> arquetipos
cd arquetipos
pnpm install
cp .env.example .env
```

Edita `.env` si necesitas otro host de Postgres. El default apunta a `localhost:5440`.

---

## 4. Infraestructura local

```bash
pnpm dev:infra
```

Levanta (vía `docker-compose.dev.yml`):

- **Postgres** `:5440` — BD `arquetipos` (y opcionalmente `arquetipos_multi`, `generic_crm`, `josanz`)
- **Redis** `:6381`
- **Kafka** `:9094`
- **Keycloak** `:9080`

Sin Redis/Kafka el backend **arranca igual** (modo degradado). Sin Postgres no hay datos reales.

---

## 5. Base de datos y datos de demo (Josanz)

```bash
pnpm josanz:bootstrap
```

Ejecuta: migrate single-tenant → `prisma generate` → seed demo (clientes, eventos).

Variables relevantes en `.env`:

| Variable | Uso |
|----------|-----|
| `DATABASE_URL` | Conexión Postgres por defecto |
| `JOSANZ_DATABASE_URL` | BD dedicada Josanz (opcional; prioridad en `josanz-api`) |
| `TENANT_MODE` | `single` para Josanz; `multi` para plantillas multi-tenant |

Detalle: [runbooks/database-migrations.md](./runbooks/database-migrations.md).

---

## 6. Levantar el producto Josanz

**Backend** (puerto 3000):

```bash
pnpm josanz-api:dev
# o: pnpm nx serve josanz-api
```

**Frontend** (puerto 4200):

```bash
pnpm nx serve josanz
```

Abre `http://localhost:4200`. Login vía Keycloak — realm `josanz` (el bootstrap de `josanz-api` fuerza `KEYCLOAK_REALM=josanz`).

---

## 7. Plantillas Arquetipos (opcional)

| Qué | Comando | Puerto |
|-----|---------|--------|
| Monolito multi | `pnpm nx serve api` | 3000 |
| Monolito single | `pnpm nx serve api-single` | 3000 |
| Gateway | `pnpm nx serve api-gateway` | 4000 |
| Microservicio clients | `pnpm nx serve clients-ms` | gRPC 50051, HTTP 4001 |
| Angular single | `pnpm nx serve angular-single` | 4200 |
| React single | `pnpm nx serve react-single` | 4201 |
| React multi | `pnpm nx serve react-multi` | 4202 |
| Angular multi | `pnpm nx serve angular-multi` | 4203 |

**Solo UI (sin Nest/Keycloak):**

```bash
pnpm mockserver                     # :4010
pnpm arq:fe:angular-single:mock     # o arq:fe:react-single:mock
```

Detalle: [runbooks/mockserver.md](./runbooks/mockserver.md).

---

## 8. Verificar que todo compila

```bash
# Preferido (cache Nx)
pnpm nx typecheck base-backend
pnpm nx test base-backend

# Producto Josanz (ejemplos)
pnpm nx typecheck josanz-backend
pnpm nx typecheck josanz

# Convenciones (o pnpm hygiene)
pnpm check:lib-layout
pnpm check:frontend-conventions
```

Si `nx` cuelga: `npx tsc -p … --noEmit` o `pnpm typecheck:affected:legacy` (ver [README § Verificación](./README.md#verificación) y [tools-layout](./runbooks/tools-layout.md)).

---

## 9. Dónde está cada cosa

```
apps/
├── arquetipos/              # Plantillas (monolito, gateway, microservicios, SPAs)
├── clientes/
│   ├── josanz/              # Producto Josanz (api + SPA)
│   └── ideauto/recalls/     # Ideauto Recalls (Nest + Next) — F84
└── productos-saas/          # Verifactu CRM, worker, etc.

libs/
├── base/                    # Kernel @base/* (backend hex, frontend por dominio, shared)
├── arquetipos/              # Thin re-exports plantilla @arquetipos/*
├── clientes/
│   ├── josanz/              # Extensión producto @josanz/*
│   └── ideauto/             # @ideauto/* (Recalls)
└── productos-saas/          # SaaS @saas/*

tools/                       # checks, dx, jest, scaffolds, mockserver, ai/ …
```

Mapa mental: [architecture/overview.md](./architecture/overview.md).  
Ideauto: [ideauto/](./ideauto/). Scripts: [runbooks/tools-layout.md](./runbooks/tools-layout.md).

---

## 10. Siguiente lectura según tu tarea

| Quiero… | Ir a |
|---------|------|
| Entender capas y por qué existen | [architecture/overview.md](./architecture/overview.md) |
| Añadir pantalla / dominio frontend | [guides/add-frontend-domain.md](./guides/add-frontend-domain.md) |
| Añadir API / dominio backend | [guides/add-backend-domain.md](./guides/add-backend-domain.md) |
| Extraer un microservicio | [guides/add-microservice.md](./guides/add-microservice.md) |
| Crear producto cliente nuevo | [clientes/nuevo-cliente-checklist.md](./clientes/nuevo-cliente-checklist.md) |
| Ideauto / Recalls | [ideauto/](./ideauto/) |
| Publicar / versionar una lib | [guides/npm-publish-and-versioning.md](./guides/npm-publish-and-versioning.md) |
| UI plantilla sin backend | [runbooks/mockserver.md](./runbooks/mockserver.md) |
| Cambiar auth, eventos, cifrado… | [adr/README.md](./adr/README.md) |
| Desplegar / incidentes | [runbooks/README.md](./runbooks/README.md) |
| Ver planes activos | [plans/README.md](./plans/README.md) |

---

## Enlaces

- [README.md](./README.md) — índice maestro
- [AGENTS.md](../AGENTS.md) — reglas para agentes IA / resumen técnico
- [SERVICES.md](../SERVICES.md) — catálogo de dominios HTTP y eventos
- [tools/README.md](../tools/README.md) — utilidades del monorepo
