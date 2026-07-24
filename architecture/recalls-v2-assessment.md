<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Recalls_v2 — assessment y por qué migrar</h1>

<p align="center">
  <b>Diagnóstico del legacy</b> · justificación de producto · gaps
</p>

<p align="center">
  <img alt="architecture" src="https://img.shields.io/badge/architecture-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-84-eighty-four-round/"><img alt="F84" src="https://img.shields.io/badge/plan-F84-14b8a6?style=flat-square" /></a>
</p>

Cuándo leerlo: antes de tocar código de migración Recalls, en kickoff de producto, o cuando alguien pregunte *“¿por qué no seguimos con Express + Next sueltos?”*.

---

## 1. Qué es Recalls_v2 hoy

Producto **Ideauto** de gestión de campañas de recall vehicular: altas de campaña, VINs, oleadas postales/telemáticas, integración **DGT** (SOAP), presupuestos/facturas, informes y panel admin.

| Pieza | Stack |
|-------|-------|
| `ideauto-server` | Express 5 · Sequelize 6 · MSSQL (`tedious`) · JWT · `soap` · Winston · PM2 · JS ESM |
| `ideauto-client` | Next 16 · React 19 · Redux Toolkit · Tailwind 4 · next-intl · Formik/Yup |
| Tooling | Dos workspaces pnpm · sin Nx · Jest parcial en server |

Snapshot de referencia (fuera del monorepo): carpeta `Recalls_v2_*` en Downloads del equipo. **No** se importa el árbol legacy al repo; se **reconstruye** sobre Arquetipos.

---

## 2. Por qué es necesario dejar de hacerlo “así”

### 2.1 Arquitectura que no escala el dominio

El backend es un **saco de ~166 services** + routers Express sin hexagonal ni CQRS. El dominio (campaña, oleada, DGT, PDF) se enreda en utilidades de ficheros, query builders y side-effects. En Arquetipos eso está prohibido por diseño ([overview](./overview.md), [ADR 0001](../adr/adr-0001-hexagonal-architecture.md), [ADR 0009](../adr/adr-0009-cqrs-nest.md)).

Seguir añadiendo features al legacy = **más acoplamiento**, no más producto.

### 2.2 Persistencia atada a un proveedor

Sequelize + MSSQL sin capa de provider. Cambiar de motor (o siquiera compartir patrones con Josanz/Verifactu en Postgres) obliga a reescribir acceso a datos. El monorepo invierte en **Prisma multi-provider** ([F83](../plans/rounds/plans-83-eighty-three-round/), [ADR 0002](../adr/adr-0002-prisma-multi-single-tenancy.md)).

### 2.3 Seguridad y auth “de producto casero”

La documentación de API marca varios endpoints de usuarios como **públicos**. La identidad depende de JWT propio + paquete externo `@ideauto/authguard-core`. El motor ya eligió **Keycloak como IdP** y API como resource server ([ADR 0005](../adr/adr-0005-jwt-vs-keycloak.md)). Migrar auth no es cosmética: es alinear el producto con el contrato de plataforma y cerrar superficie de ataque.

### 2.4 Frontend sin contrato de capas

El client organiza UI en atomic design (`atoms/molecules/organisms`) y habla HTTP vía RTK Query desde `store/api`. Eso **no** es el contrato `api → data-access → features ← ui` ([ADR 0006](../adr/adr-0006-frontend-layering.md), [domain architecture](../frontend/frontend-domain-architecture-contract.md)). Atomic design puede vivir en el design system; **no** sustituye el layout de dominio.

### 2.5 Operación y calidad

| Señal | Lectura |
|-------|---------|
| ~52 tests / ~520 archivos BE | Cobertura insuficiente para DGT/oleadas |
| Jobs con `node-schedule` en el API | Un restart mata o duplica trabajo |
| 59 migraciones Sequelize + SQL suelto | Sin schema único ni parity checks |
| Cascada de `pnpm.overrides` | Deuda de supply-chain reactiva |
| Descripción npm aún “MongoDB” | Documentación y realidad desalineadas |

### 2.6 Coste estratégico

Mientras Recalls viva fuera del monorepo:

- no reutiliza `@base/audit`, documents, pagination, RBAC, Storybook, CI gates;
- cada mejora de plataforma (F83, native-ui, coverage) **no llega** al producto;
- el equipo mantiene **dos culturas** de ingeniería.

---

## 3. Matriz comparativa

| Dimensión | Legacy | Destino monorepo |
|-----------|--------|------------------|
| Apps | Dos repos | `apps/productos-saas/recalls/*` composition roots |
| Libs | Ninguna compartida | `@saas/*` + `@base/*` |
| Backend | Express monolito | Nest + hex + módulos por dominio |
| ORM | Sequelize MSSQL | Prisma + adapter MSSQL (F83) |
| FE layering | Atomic + RTK | 4 capas + shell lazy |
| Auth | JWT + authguard-core | Keycloak / Nest guards |
| Jobs | In-process | Worker / cola |
| CI | Scripts locales | `nx affected` + Cloud |
| Boundaries | No | `layer:productos-saas` ESLint |

---

## 4. Workstreams (P0 → P2)

| Prio | Qué | Por qué |
|------|-----|---------|
| P0 | Auth + Prisma MSSQL + scaffold apps | Seguridad + base del strangler |
| P1 | Campaigns/waves + DGT adapter | Core negocio + riesgo externo |
| P2 | Budgets/invoices/PDF + reports + workers | Paridad operativa |

Detalle de ejecución: [runbooks/recalls-migration.md](../runbooks/recalls-migration.md).  
Strategy: [recalls-migration-strategy.md](./recalls-migration-strategy.md).  
Mapping: [recalls-domain-mapping.md](./recalls-domain-mapping.md).

---

## 5. Qué **no** implica este assessment

- No obliga a apagar legacy mañana (strangler, [ADR 0013](../adr/adr-0013-recalls-strangler-migration.md)).
- No copia carpetas legacy al monorepo.
- No mezcla Recalls con fiscal Verifactu ni con ERP Josanz.

---

## Enlaces

- Ronda [F84](../plans/rounds/plans-84-eighty-four-round/)
- [productos-saas-extends-base.md](../productos-saas/productos-saas-extends-base.md)
- [framework-decision-guide.md](./framework-decision-guide.md) — Nest default; Next opt-in
