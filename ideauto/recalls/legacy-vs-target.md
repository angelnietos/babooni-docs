<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="48" alt="Arquetipos" />
</p>

<h1 align="center">Legacy vs destino</h1>

<p align="center">
  <a href="./README.md"><img alt="Recalls" src="https://img.shields.io/badge/hub-Recalls-0f766e?style=flat-square" /></a>
</p>

---

## Matriz

| Dimensión | Recalls_v2 (legacy) | Destino monorepo |
|-----------|---------------------|------------------|
| Colocación | `ideauto-server` + `ideauto-client` | `apps/clientes/ideauto/recalls` + `libs/clientes/ideauto` |
| npm | sin scope monorepo | `@ideauto/*` · `layer:clientes` |
| Backend | Express 5 monolito ESM (JS) | NestJS + hex + CQRS (TS) |
| Persistencia | Sequelize + MSSQL (`tedious`) | Prisma + adapter MSSQL (**F83**) |
| Auth | JWT + `@ideauto/authguard-core` | Keycloak / Nest guards |
| Frontend | Next + RTK + atomic UI | Next + `api → data-access → features` |
| Jobs | `node-schedule` in-process | Worker / cola |
| Tooling | pnpm suelto + PM2 | Nx + affected + Cloud |
| Tests | ~52 tests / ~520 archivos BE | Jest en libs + gates |
| Multi-tenant | No | Opcional vía ADR 0002 |
| Auditoría | `CampaignsHistory` ad-hoc | `@base/audit-*` donde aplique |

---

## Inventario legacy (referencia)

| Pieza | Dato |
|-------|------|
| Modelos Sequelize | 24 |
| Routers API | 17 |
| Migraciones | 59 |
| Services BE | ~166 |
| Snapshot | fuera del monorepo (no commitear) |

---

## Qué no se copia “tal cual”

- Carpetas `atoms/molecules/organisms` como arquitectura de dominio  
- `queryBuilder.js` genérico  
- Repos duplicados  
- Endpoints usuarios públicos  
- Lógica DGT repartida en utils sueltos  

---

## Enlaces

- [why-migrate.md](./why-migrate.md)  
- [domain-map.md](./domain-map.md)
