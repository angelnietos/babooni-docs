<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F73 — Arquitectura Isomórfica Canónica & Native UI Cross-Framework</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

F73-A1 (Isomorphic Shared Contracts) y F73-C1 (DX Governance) completados.
F73-B1 (ViewModels) piloto `clients` completado.
F73-A2 (Native UI SSR) no iniciado.

> Apertura 2026-07-23. Eje: **Contratos Isomórficos Canónicos** (DTOs + Validación Zod compartida Backend/Frontend);
> **Native UI Cross-Framework** (Compatibilidad SSR Lit CE en Next.js + RN bridge);
> **ViewModels Portables** (Vanilla TS logic reusable en los 5 stacks).

## Contexto

Tras la consolidación de la estructura física de features (`layout/`, `pages/`, `components/`, `logic/`) en la Ronda F72, la Ronda F73 ataca la **redundancia de datos y contratos entre Frontend y Backend**.

Actualmente:
- DTOs como `ClientDto` están duplicados en cada paquete `@base/<fw>-<domain>-api` y en los servicios NestJS.
- La validación difiere entre esquemas Zod en FE y `class-validator` en Nest.
- Lit Custom Elements requieren puentes dedicados para renderizado en servidor (Next.js SSR) y entornos móviles nativos (React Native).

## Hitos F73

| ID | Plan | Descripción | Estado |
|----|------|-------------|--------|
| F73-A1 | [Isomorphic Shared Contracts](1750000301000-f73-isomorphic-shared-contracts.md) | Contratos de dominio isomórficos (DTOs + Zod) entre NestJS y los 5 frontends | **Completado** |
| F73-A2 | [Cross-Framework Native UI](1750000302000-f73-cross-framework-native-ui.md) | Puente SSR de Lit CE en Next.js y adaptadores nativos en React Native | No iniciado |
| F73-B1 | [Shared ViewModels Architecture](1750000303000-f73-shared-viewmodels-architecture.md) | Controllers/ViewModels en TypeScript agnóstico reutilizables por React/Angular/RN | **Piloto clients completado** |
| F73-C1 | [DX & Automation Governance](1750000304000-f73-monorepo-dx-automation.md) | Generadores y validadores CI para gobernanza de contratos isomórficos | **Completado** |

## Entregables cerrados F73

- [x] A1: DTOs centralizados en `@base/shared/src/lib/domains/<domain>/<domain>.dto.ts`.
- [x] A1: Validación Zod en `@base/shared/src/lib/validation/<domain>.ts` y `<domain>.schema.ts`.
- [x] A1: Todos los api packages reexportan desde `@base/shared` sin redefiniciones locales.
- [x] C1: `check-domain-conventions.mjs` detecta DTOs locales en api packages.
- [x] B1: `ClientsController.ts` (Vanilla TS) + `useClientsController` funcionando.

## Siguiente

- [Ronda F74](../plans-74-seventy-four-round/) — Extensión multi-dominio completa.
