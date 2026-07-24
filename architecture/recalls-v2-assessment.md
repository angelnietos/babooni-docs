<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Recalls_v2 — assessment y por qué migrar</h1>

<p align="center">
  <b>Diagnóstico del legacy</b> · destino <code>clientes/ideauto/recalls</code>
</p>

<p align="center">
  <img alt="architecture" src="https://img.shields.io/badge/architecture-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-84-eighty-four-round/"><img alt="F84" src="https://img.shields.io/badge/plan-F84-14b8a6?style=flat-square" /></a>
</p>

Cuándo leerlo: kickoff Ideauto Recalls, o cuando alguien pregunte *“¿por qué no seguimos con Express + Next sueltos?”* / *“¿va a SaaS?”*.

---

## 1. Qué es Recalls_v2 hoy

Producto **Ideauto** de campañas de recall vehicular: VINs, oleadas postales/telemáticas, **DGT** (SOAP), presupuestos/facturas, informes y admin.

| Pieza | Stack |
|-------|-------|
| `ideauto-server` | Express 5 · Sequelize · MSSQL · JWT · SOAP · PM2 · JS |
| `ideauto-client` | Next 16 · React 19 · Redux Toolkit · Tailwind 4 |

Snapshot fuera del monorepo. Se **reconstruye** aquí; no se importa el árbol legacy.

---

## 2. Por qué migrar (y a dónde)

### Destino correcto

| | |
|--|--|
| Apps | `apps/clientes/ideauto/recalls/{backend,frontend}` |
| Libs | `libs/clientes/ideauto/` → **`@ideauto/*`** |
| Layer | `layer:clientes` (mismo patrón que Josanz) |
| Kernel | `@base/*` |

### Destino **incorrecto**

| No | Por qué |
|----|---------|
| `apps/productos-saas/recalls` / `@saas/*` | SaaS es otra capa (Verifactu, etc.). Ideauto es **cliente**. |
| Mezclar con `@josanz/*` | Marcas distintas; boundaries. |

Guía: [nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md).

### Por qué dejar el legacy

1. **Arquitectura** — ~166 services planos sin hex/CQRS.
2. **Persistencia** — Sequelize MSSQL-only; el monorepo usa Prisma portable (F83).
3. **Seguridad** — auth casera + endpoints usuarios documentados como públicos.
4. **FE** — atomic design ≠ contrato `api → data-access → features`.
5. **Operación** — jobs in-process, migraciones ad-hoc, sin Nx boundaries.
6. **Estrategia** — fuera del monorepo no reutiliza `@base` ni gates CI.

---

## 3. Matriz

| Dimensión | Legacy | Destino |
|-----------|--------|---------|
| Colocación | 2 repos | `clientes/ideauto/recalls` |
| npm | — | `@ideauto/*` |
| Backend | Express | Nest + hex |
| ORM | Sequelize | Prisma + MSSQL (F83) |
| FE | Next + RTK | Next + 4 capas |
| Auth | JWT + authguard-core | Keycloak / Nest guards |

---

## 4. Workstreams

| Prio | Qué |
|------|-----|
| P0 | Scaffold Ideauto + auth + Prisma MSSQL |
| P1 | Campaigns/waves + DGT |
| P2 | Budgets/PDF + reports + workers |

→ [runbooks/recalls-migration.md](../runbooks/recalls-migration.md) · [recalls-domain-mapping.md](./recalls-domain-mapping.md) · [ADR 0013](../adr/adr-0013-recalls-strangler-migration.md)

---

## Enlaces

- Ronda [F84](../plans/rounds/plans-84-eighty-four-round/)
- [framework-decision-guide.md](./framework-decision-guide.md)
