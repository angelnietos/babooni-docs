<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F65-B1 — Carry: validación isomórfica</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F65" src="https://img.shields.io/badge/round-F65-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Retomar [isomorphic-validation-defer.md](../../../frontend/isomorphic-validation-defer.md)
(F64-D1). Piloto clients Create/Update en `@base/shared` + adapters FE.

## Entregables

1. [ADR 0012](../../../adr/adr-0012-isomorphic-validation.md) — class-validator SoT; Zod → F66.
2. Predicates `validateCreateClient` / `validateUpdateClient` en `@base/shared`.
3. Uso en panels Angular/React clients (warning toast si inválido).

## Criterios de aceptación

- [x] Piloto verde **o** defer F66 explícito (motivo + owner).
- [x] ADR + predicates; Zod kit defer F66 documentado.

## Verificación

Documentar decisión en hub F65 + defer note + ADR README.

## Depende de

Capacidad; no bloquea A2–A4.
