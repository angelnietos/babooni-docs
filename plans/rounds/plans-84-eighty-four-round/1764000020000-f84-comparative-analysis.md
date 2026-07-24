<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F84-A1 — Por qué migrar · análisis comparativo</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F84" src="https://img.shields.io/badge/round-F84-14b8a6?style=flat-square" /></a>
  <a href="../../../architecture/recalls-v2-assessment.md"><img alt="biblia" src="https://img.shields.io/badge/biblia-assessment-0f766e?style=flat-square" /></a>
</p>

## Estado

**completado (contenido en biblia)** · 2026-07-24

> Canónico para humanos: [`docs/architecture/recalls-v2-assessment.md`](../../../architecture/recalls-v2-assessment.md).  
> Este plan es el **ticket de ronda** + checklist; no duplicar tablas largas aquí.

---

## Objetivo

Justificar con evidencia por qué **dejar de construir “a lo Recalls_v2”** y priorizar workstreams P0/P1/P2 para el strangler.

---

## Tesis (el porqué en una frase)

> Recalls_v2 acopla dominio de negocio crítico (DGT, oleadas, VINs, facturación) a un **monolito Express/JS sin capas**, un **ORM de un solo proveedor** y un **frontend sin contrato de dominio** — exactamente el anti-patrón que el monorepo Arquetipos ya resolvió con Nest hex, Prisma portable, 4 capas FE y gates CI.

Seguir así implica:

1. **Seguridad reactiva** — endpoints de usuarios documentados como públicos; auth via paquete externo; overrides npm interminables.
2. **Imposibilidad de reutilizar** — no hay `@base/*`; cada feature inventa su servicio, su query builder, su PDF.
3. **Riesgo operativo** — jobs in-process, migraciones Sequelize ad-hoc, sin `nx affected`.
4. **DX rota** — dos repos, dos lockfiles, sin module boundaries; onboarding = arqueología.

---

## Matriz side-by-side (resumen)

| Dimensión | Recalls_v2 (hoy) | Destino Arquetipos |
|-----------|------------------|--------------------|
| Backend | Express 5 monolito ESM | NestJS modular + hex + CQRS |
| Persistencia | Sequelize + MSSQL only | Prisma + adapter MSSQL (**F83**) |
| Tipado BE | JavaScript + JSDoc parcial | TypeScript estricto |
| Frontend | Next + RTK Query + atomic UI | Next (opt-in) + `api → data-access → features` |
| Auth | JWT propio + `@ideauto/authguard-core` | Keycloak / guards Nest (ADR 0005) |
| Jobs | `node-schedule` en el API | Worker / cola desacoplada |
| Tooling | pnpm suelto + PM2 | Nx monorepo + Cloud cache |
| Tests | ~52 `*.test.js` / 166 services | Jest en libs + coverage gates |
| Multi-tenant | No | Schema multi opcional (ADR 0002) |
| Auditoría / PII | Historial ad-hoc (`CampaignsHistory`) | `@base/audit-*` + encryption ADR 0003 |

---

## Deuda técnica que **no** se “parchea”

Estas no son mejoras cosméticas; son motivos para **no seguir el patrón legacy**:

| Deuda | Por qué duele | Mitigación en destino |
|-------|---------------|------------------------|
| Services flat (~166) | Lógica de campaña/DGT/PDF sin ports | Un módulo Nest por dominio + ports |
| Query builder casero | SQL implícito, difícil de testear | Prisma + repositorios |
| File paths en disco por campaña | Acoplamiento FS + negocio | Storage port + `@saas/files-*` |
| SOAP DGT embebido en services | Sin seam para mock/parallel-run | `dgt-api` adapter aislado |
| Redux + fetch en páginas | Estado HTTP en UI | `data-access` facades |
| Atomic design sin dominio | Componentes no mapean a features | Feature folders + UI package |
| `package.json` BE aún dice “MongoDB” | Señal de deuda documental/histórica | Docs + schema únicos |

---

## Gaps de features (no son blockers de migración)

| Capacidad legacy | Destino |
|------------------|---------|
| SOAP DGT (recalls, addresses, deregistrations) | `@saas/dgt-*` + client `soap` |
| PDFs / DOCX / XLSX / Handlebars | Utilities `@base` / documents + templates producto |
| Oleadas postales + telemáticas | Subdominio `waves` bajo `campaigns` |
| Informes renting / VIN / certificates | `@saas/reports-*` |
| Subida masiva VINs (CSV/XLSX/TXT) | Nest `FilesInterceptor` + parsers tipados |
| Mail (Mailjet / nodemailer) | Puerto notificaciones Nest |

---

## Workstreams priorizados

| Prio | Workstream | Por qué primero |
|------|------------|-----------------|
| **P0** | Auth + users + perfiles | Cierra agujeros de seguridad; desbloquea el resto |
| **P0** | Prisma introspect MSSQL + F83 adapters | Sin DB no hay strangler |
| **P0** | Scaffold apps/libs recalls | Composition roots Nx |
| **P1** | Campaigns + waves + VIN files | Core de negocio |
| **P1** | DGT adapter + parallel-run | Riesgo externo máximo |
| **P2** | Budgets / invoices / PDFs | Dependientes de campaña |
| **P2** | Reports + admin dashboard + jobs | Valor operativo, menor riesgo cutover |

---

## Acciones / criterios

- [x] Inventario legacy (modelos, rutas, stack, tamaño).
- [x] Matriz comparativa publicada en biblia.
- [x] Lista P0/P1/P2.
- [ ] Producto confirma prioridades antes de M2 (campaigns).

## Enlaces

- [README F84](./README.md)
- [Assessment (biblia)](../../../architecture/recalls-v2-assessment.md)
- [F84-B1 strategy](./1764000021000-f84-migration-strategy.md)
