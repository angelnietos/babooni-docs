<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F76 — Canonical Domain Architecture Consolidation & Public API Cleanup</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-75-seventy-five-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

En ejecución · Parcialmente completado

> Apertura 2026-07-23. Eje: **Despliegue Físico de la Arquitectura de 8 Capas por Dominio**, eliminación de Barrels Gigantes ("God Barrels"), limpieza de reexports transversales prohibidos, y completitud de configuraciones de `test` y `lint` en todas las librerías del monorepo (Angular, Ionic, React, Next.js, React Native).

## Progreso

- [x] F75-F1 — God barrels & transversal re-exports eliminados de Angular; `tools/checks/check-public-api-barrels.mjs` verde.
- [x] F75-F2 — `test` y `lint` targets agregados a 30 base Angular domain libs; `jest.config.ts` y `tsconfig.spec.json` generados.
- [ ] F76-A1 Next base clients (planeado)
- [x] F76-A2.1 Base Angular: clients (estructura base + api barrel limpia)
- [ ] F76-A2.2..F76-A2.10 Base/Arquetipos Angular resto dominios
- [ ] F76-A3 React completo (base + arquetipos)
- [ ] F76-A4 Next completo (resto base + arquetipos)
- [ ] F76-A5 Ionic completo
- [ ] F76-A6 React Native completo
- [x] F76-B1 Barrel & Public API Cleanup — Angular completado
- [x] F76-B2 Hardening test/lint — Angular base completado
- [x] F76-B3 `check-public-api-barrels.mjs` — script verde

## Contexto

La Ronda F75 definió los contratos de arquitectura de 8 capas y las reglas estrictas de API Pública (prohibiendo que una librería `api` reexporte `data-access`, Guards, Componentes o Stores). La Ronda F76 ejecuta el rollout físico completo en todo el monorepo y cierra todas las tareas pendientes de la Ronda F75.

## Objetivos Clave de la Ronda F76

1. **Rollout de 8 Capas**: Crear/migrar paquetes Nx independientes (`domain`, `api`, `data-access`, `features`, `shell`, `ui`, `shared`, `testing`) en todos los dominios base y arquetipos de Angular, React, Next.js, Ionic y React Native.
2. **Limpieza de "God Barrels"**: Refactorizar archivos `index.ts` que realizan `export * from '@base/*'` cruzados (ejemplos: `libs/base/frontend/angular/domains/clients/api/src/index.ts`, `libs/base/frontend/angular/platform/kernel/index.ts`).
3. **Hardening de Configuración (`test` & `lint`)**: Añadir targets de Jest/Vitest (`jest.config.ts`) y ESLint (`.eslintrc.json`) en el `project.json` de cada librería donde falten.
4. **Gates de CI Integrados**: Activar `check-frontend-conventions.mjs`, `check-domain-conventions.mjs` y `check-public-api-barrels.mjs` en el pipeline de CI.

## Documentos de la Ronda F76

- **[F76 Rollout Plan](1760000000000-f76-canonical-domain-architecture-rollout.md)**: Plan detallado de migración dominio por dominio, eliminación de barrels y auditoría de tests/linting.

## Predecesora / Siguiente

- Predecesora: [F75](../plans-75-seventy-five-round/)
- Siguiente: TBD
