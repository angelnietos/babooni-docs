<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F75 — Frontend Canonical Domain Architecture (Cross-Framework)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-74-seventy-four-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

> Apertura 2026-07-23. Eje: **Arquitectura Canónica Frontend Multi-Framework**, homogeneización de la estructura de librerías por dominio para **Angular, Ionic (Angular/React), React, Next.js y React Native**, contrato de dependencias entre capas, generadores atomizados y refactorización de librerías con fronteras inconsistentes.

## Contexto

Las rondas anteriores (F35, F72, F74) establecieron la separación conceptual entre `api`, `data-access` y `features`, pero la implementación física quedó inconsistente o restringida a un único framework (e.g. Next.js). En la práctica surgieron vicios arquitectónicos como:
- Importar `@base/next-users-api` dentro de `data-access`.
- Mezclar llamadas HTTP directamente en componentes o en la capa `data-access`.
- Ausencia de la capa pura `domain/` agnóstica a frameworks.
- Falta de convención clara para UI compartida del dominio (`ui/`), layouts de entrada (`shell/`) y testing helpers (`testing/`).

La Ronda F75 eleva la arquitectura frontend a **Estándar de Plataforma** para todos los dominios y frameworks del monorepo.

## Arquitectura Canónica del Dominio Frontend

Cada dominio frontend (`users`, `auth`, `roles`, `settings`, `clients`, `billing`, `inventory`, `projects`, `tenants`) tendrá la misma organización en 8 capas conceptuales:

```text
[domain]/
├── domain/        # Lógica de negocio pura (0 dependencias de framework)
├── api/           # HTTP/GraphQL/SDKs, DTOs, Mappers (sin estado)
├── data-access/   # Server State (TanStack Query) + Client State (Signals/Zustand)
├── features/      # Casos de uso de pantalla, páginas, componentes de feature
├── shell/         # Layouts, navegación, providers de entrada al dominio
├── ui/            # Componentes, directivas y pipes presentacionales de dominio
├── shared/        # Utilidades, constantes y helpers agnósticos
└── testing/       # Mocks, fixtures, harnesses y test utilities
```

## Flujo de Dependencias Permitidas

```text
features
   ↓
data-access
   ↓
api
   ↓
domain

ui ─────► domain
shell ──► features, ui, shared
shared ─► consumido por todas las capas
```

*Regla de Oro: Las dependencias son siempre descendentes. Ninguna capa inferior puede importar de una capa superior (ej. `api` JAMÁS importa de `data-access`).*

## Hitos F75

| ID | Plan | Descripción | Estado |
|----|------|-------------|--------|
| F75-A1 | [Canonical Architecture](1750000501000-f75-canonical-domain-architecture.md) | Especificación estructural canónica de 8 capas por dominio | Listo para ejecutar |
| F75-A2 | [Layer Contracts](1750000502000-f75-layer-responsibilities-and-contracts.md) | Contrato explícito de responsabilidades y fronteras por capa | Listo para ejecutar |
| F75-B1 | [State Standard](1750000503000-f75-state-management-standard.md) | Estandarización de estado: TanStack Query + Signals (Angular) / Zustand (React/Next/RN) | Listo para ejecutar |
| F75-C1 | [Generators](1750000504000-f75-generators-and-scaffolding.md) | Generador `generate-domain` y subgeneradores de capa atomizados | Listo para ejecutar |
| F75-D1 | [CI Validations](1750000505000-f75-automated-validations-and-ci-gates.md) | Reglas de linting y gates de CI para auditar fronteras de arquitectura | Listo para ejecutar |
| F75-E1 | [Cross-Framework Migration](1750000506000-f75-cross-framework-migration.md) | Plan de refactorización de librerías existentes a la arquitectura F75 | Listo para ejecutar |
| F75-F1 | [Public API & Barrel Cleanup Contract](1750000507000-f75-public-api-and-barrel-cleanup-contract.md) | Prohibición de "God Barrels" y reexports transversales cruzados | Cerrado |

## Criterios de Aceptación (Ronda F75)

- [x] A1: Estructura de 8 capas definida y documentada para Angular, Ionic, React, Next.js y React Native.
- [x] A2: Contrato formal publicado prohibiendo importaciones circulares o ascendentes (e.g. `api` -> `data-access`).
- [x] B1: Estrategia de Server State (TanStack Query) y Client State (Signals / Zustand) integrada en todas las plantillas.
- [x] C1: Generadores `generate-domain`, `generate-api`, `generate-data-access`, `generate-feature`, `generate-ui`, `generate-shell` operativos en `tools/scaffolds/`.
- [x] D1: `check-frontend-conventions.mjs` valida reglas de fronteras de capa en CI bloqueando importaciones prohibidas.
- [x] E1: Refactorización de `libs/base/frontend/next/users/*` y equivalentes en Angular/React alineada con el contrato F75.
- [x] F1: Eliminación de "God Barrels" e imposición de `check-public-api-barrels.mjs` en CI para bloquear `export * from '@base/*'` transversales.
- [x] F2: Configuración de targets de `test` y `lint` completada en `project.json` para todas las librerías creadas.

## Predecesora / Siguiente

- Predecesora: [F74](../plans-74-seventy-four-round/)
- Siguiente: [F76](../plans-76-seventy-six-round/)
