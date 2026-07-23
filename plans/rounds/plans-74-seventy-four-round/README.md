<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F74 — Extensión Isomórfica Total & Gobernanza Multi-Dominio</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-73-seventy-three-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F73-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

> Apertura 2026-07-23. Eje: **Extensión de la capa de contratos isomórficos a todos los dominios**,
> estandarización del patrón ViewModel en `logic/` por dominio, endurecimiento del generator
> `gen-domain.mjs` y bloqueo de PRs con DTOs duplicados.

## Contexto

La Ronda F73 estableció el patrón canónico:
- DTOs y validación Zod viven en `libs/base/shared/src/lib/{contracts,validation}/`.
- Los paquetes `api` deben reexportar desde `@base/shared` sin redefinir tipos.
- `check-domain-conventions.mjs` bloquea DTOs locales en api packages.
- El dominio piloto `clients` tiene `ClientsController` (Vanilla TS) + `useClientsController` (React).

Faltan **7 dominios** (auth, users, billing, inventory, projects, roles, settings, tenants)
sin contratos isomórficos ni ViewModels portables. También falta el generator `gen-domain.mjs`
y la verificación multi-dominio en CI.

## Hitos F74

| ID | Plan | Descripción | Estado |
|----|------|-------------|--------|
| F74-A1 | [Full Isomorphic Coverage](1750000401000-f74-full-isomorphic-coverage.md) | Zod schemas + predicados para todos los dominios | Listo para ejecutar |
| F74-A2 | [NestJS Contract Integration](1750000402000-f74-nestjs-contract-integration.md) | Pipes y validadores que consumen `@base/shared` desde Nest | Listo para ejecutar |
| F74-B1 | [Cross-Domain ViewModel Standardization](1750000403000-f74-viewmodel-standardization.md) | ClientsController replicado/adaptado por dominio restante | Listo para ejecutar |
| F74-C1 | [Generator & CI Hardening](1750000404000-f74-generator-ci-hardening.md) | `gen-domain.mjs` scaffolding completo + gates CI | Listo para ejecutar |

## Criterios de Aceptación (Ronda F74)

- [ ] A1: F74 tiene Zod schemas para TODOS los dominios (clients, users, auth, billing, inventory, projects, roles, settings, tenants).
- [ ] A2: Los controladores NestJS de ≥3 dominios usan Pipes que consumen schemas de `@base/shared`.
- [ ] B1: ≥5 dominios tienen `logic/Controllers.ts` (Vanilla TS) importados simultáneamente por dos frameworks distintos.
- [ ] C1: `node tools/scaffolds/gen-domain.mjs <domain>` genera estructura completa; `check-domain-conventions` y `check-frontend-conventions` pasan verde en CI.

## Predecesora / Siguiente

- Predecesora: [F73](../plans-73-seventy-three-round/)
- Siguiente: TBD
