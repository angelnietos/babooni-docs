# Primer día — arranque local

Guía para un desarrollador nuevo. Tiempo estimado: **30–45 minutos** hasta ver Josanz funcionando en local.

> Si solo quieres orientarte sin levantar nada, lee primero [architecture/overview.md](./architecture/overview.md).

---

## 1. Qué es este repositorio (en una frase)

Un **monorepo Nx** con un kernel reutilizable (`@base/*`), plantillas de referencia (`@arquetipos/*`), productos de cliente (`@josanz/*`, etc.) y SaaS (`@saas/*`). Las **apps** componen libs; las **libs** no eligen cómo se despliegan (monolito, microservicio, BD compartida o dedicada).

---

## 2. Prerrequisitos

| Herramienta | Versión orientativa |
|-------------|---------------------|
| Node.js | 20+ (ver `.nvmrc` si existe) |
| pnpm | 9+ (`packageManager` en `package.json`) |
| Docker Desktop | Para Postgres, Redis, Kafka, Keycloak |

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

---

## 8. Verificar que todo compila

```bash
# Backend kernel
npx tsc -p libs/base/backend/tsconfig.lib.json --noEmit
npx jest --config libs/base/backend/jest.config.ts

# Producto Josanz
npx tsc -p libs/clientes/josanz/backend/tsconfig.lib.json --noEmit
npx tsc -p apps/clientes/josanz/frontend/josanz/tsconfig.app.json --noEmit

# Convenciones (rápido)
node tools/scripts/check-frontend-conventions.mjs
node tools/scripts/check-lib-layout.mjs
```

Si `nx` cuelga, usa `tsc` directamente (ver [README § Verificación](./README.md#verificación)).

---

## 9. Dónde está cada cosa

```
apps/
├── arquetipos/          # Plantillas (monolito, gateway, microservicios, SPAs)
├── clientes/josanz/     # Producto Josanz (api + SPA)
└── productos-saas/      # Verifactu CRM, worker, etc.

libs/
├── base/                # Kernel @base/* (backend hex, frontend por dominio, shared)
├── arquetipos/          # Thin re-exports plantilla @arquetipos/*
├── clientes/josanz/     # Extensión producto @josanz/*
└── productos-saas/      # SaaS @saas/*
```

Mapa mental completo: [architecture/overview.md](./architecture/overview.md).

---

## 10. Siguiente lectura según tu tarea

| Quiero… | Ir a |
|---------|------|
| Entender capas y por qué existen | [architecture/overview.md](./architecture/overview.md) |
| Añadir pantalla / dominio frontend | [guides/add-frontend-domain.md](./guides/add-frontend-domain.md) |
| Añadir API / dominio backend | [guides/add-backend-domain.md](./guides/add-backend-domain.md) |
| Extraer un microservicio | [guides/add-microservice.md](./guides/add-microservice.md) |
| Crear producto cliente nuevo | [clientes/nuevo-cliente-checklist.md](./clientes/nuevo-cliente-checklist.md) |
| Cambiar auth, eventos, cifrado… | [adr/README.md](./adr/README.md) |
| Desplegar / incidentes | [runbooks/README.md](./runbooks/README.md) |

---

## Enlaces

- [README.md](./README.md) — índice maestro
- [AGENTS.md](../AGENTS.md) — reglas para agentes IA / resumen técnico
- [SERVICES.md](../SERVICES.md) — catálogo de dominios HTTP y eventos
