<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F84 — Recalls_v2 → Arquetipos migration</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-83-eighty-three-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F83-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Activa** · apertura 2026-07-24

> Eje: evaluar y, en su caso, migrar el proyecto legacy `Recalls_v2_2016-07-24` (ideauto-server + ideauto-client) al stack arquetipos. La ronda cubre comparativa, mapeo de dominio, strategy y execution plan.

## Contexto

`Recalls_v2` es un proyecto active/legacy:

- **Backend**: Express 5 + Sequelize (MSSQL) + JWT, monolito ESM sin capas.
- **Frontend**: Next.js 16 + React 19 + Redux Toolkit + Tailwind v4.
- **Sin tooling Nx**, sin barrel packages, sin layering frontend.
- **Dominio**: campañas de recall vehicular, oleadas postales/telemáticas, integración DGT, presupuestos, facturación.
- **Stack atado a MSSQL** con `tedious`, sin capa de abstracción de proveedor.

Arquetipos ofrece un stack estructurado, tipado y con convenciones que resuelven deuda técnica de Recalls_v2:

1. Nx + TypeScript estricto + coverage.
2. NestJS + Prisma multi/single tenant (ya soporta MSSQL via F83).
3. Capas frontend `api → data-access → features` con Angular/React/Next.
4. Base reutilizable (`@base/*`) para auth, RBAC, documentos, auditoría.
5. Herramientas CI, Storybook, visual testing, SCA/SBOM.

## Objetivos clave

1. Comparativa detallada (features, arquitectura, deuda técnica, DX).
2. Mapeo 1:1 de dominio Recalls_v2 → arquetipos (dominios candidatos: campaigns, waves, budgets, invoices, dgt, clients).
3. Strategy de migración: big-bang por dominio vs strangler.
4. Plan de ejecución con milestones y gates verdes.
5. Estimación de esfuerzo y riesgos.

## Planes detallados

| ID | Plan | Foco |
|----|------|------|
| F84-A1 | [Comparative analysis](1764000020000-f84-comparative-analysis.md) | Matriz side-by-side + deuda técnica |
| F84-B1 | [Migration strategy](1764000021000-f84-migration-strategy.md) | Strangler vs big-bang, milestones, rollback |
| F84-C1 | [Domain mapping](1764000022000-f84-domain-mapping.md) | Recalls_v2 entities → arquetipos domains/packages |
| F84-D1 | [Technical execution plan](1764000023000-f84-technical-execution.md) | Paso a paso por dominio, DB migration, DGT SOAP adapter |
| F84-E1 | [Docs, gates y cierre](1764000024000-f84-docs-gates-close.md) | Hub planes + ADR + execution checklist |

## Checklist de cierre F84

- [ ] F84-A1 informe comparativo publicado
- [ ] F84-B1 strategy aprobada (strangler recomendada)
- [ ] F84-C1 mapeo dominio firmado por producto
- [ ] F84-D1 plan técnico con milestones y criterios de done
- [ ] F84-E1 docs + hub; **F84 cerrada**

## Predecesora / Siguiente

- Predecesora: [F83](../plans-83-eighty-three-round/) (activa)
- ADR: F84-B1 (opcional)
- Proyecto legacy: `C:\Users\amuni\Downloads\Recalls_v2_2016-07-24`
- Rutas frontend legacy: `apps/productos-saas/` (verifactu-platform) + shells `@saas/*`
