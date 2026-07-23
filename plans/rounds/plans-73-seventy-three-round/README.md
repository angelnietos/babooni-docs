<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F73 — Arquitectura Isomórfica Canónica & Native UI Cross-Framework</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

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
| F73-A1 | [Isomorphic Shared Contracts](1750000301000-f73-isomorphic-shared-contracts.md) | Contratos de dominio isomórficos (DTOs + Zod) entre NestJS y los 5 frontends | Listo para ejecutar |
| F73-A2 | [Cross-Framework Native UI](1750000302000-f73-cross-framework-native-ui.md) | Puente SSR de Lit CE en Next.js y adaptadores nativos en React Native | Listo para ejecutar |
| F73-B1 | [Shared ViewModels Architecture](1750000303000-f73-shared-viewmodels-architecture.md) | Controllers/ViewModels en TypeScript agnóstico reutilizables por React/Angular/RN | Listo para ejecutar |
| F73-C1 | [DX & Automation Governance](1750000304000-f73-monorepo-dx-automation.md) | Generadores y validadores CI para gobernanza de contratos isomórficos | Listo para ejecutar |

## Criterios de Aceptación (Ronda F73)

- [ ] A1: Eliminada la duplicación de DTOs entre NestJS y Frontends; `@base/shared-contracts` centraliza DTOs y validación Zod.
- [ ] A2: Lit Custom Elements funcionan sin fallos de SSR/Webpack en Next.js y disponen de fallback limpio en React Native.
- [ ] B1: Demostrado al menos 1 ViewModel puro importado simultáneamente por una app Angular y una app React/Next.
- [ ] C1: `check-domain-conventions.mjs` y `check-frontend-conventions.mjs` verifican la paridad isomórfica de contratos.

## Predecesora / Siguiente

- Predecesora: [F72](../plans-72-seventy-two-round/)
- Siguiente: TBD
