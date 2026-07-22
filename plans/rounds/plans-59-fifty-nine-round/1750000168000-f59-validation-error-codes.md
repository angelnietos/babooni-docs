<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F59-D1 — Problem+JSON / códigos error validación unificados</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F60-E1 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Estabilizar el **contrato de error** cuando falla validación: mismos códigos /
shape para Nest → FE (Angular/React/SaaS), reutilizable por dominio y app.

## Entregables

1. Convención: campo `code` (p. ej. `VALIDATION_FAILED`, `FIELD_INVALID`) +
   `details: { field, rule, message }[]` (RFC7807-ish o extensión del body Nest
   actual).
2. Helper isomórfico o BE-only tipado en shared para construir/parsear.
3. Actualizar mapper FE (A3) para preferir `details[]`.
4. Doc corta en runbook API / guía validación.

## Criterios de aceptación

- [ ] Piloto clients (A2) emite el envelope nuevo (o dual-write compatible).
- [ ] Al menos un test de contrato (unit o e2e-spec) del shape.
- [ ] Apps legacy siguen parseando `message` string (compat).

## Relación

Complementa A1–A3; puede ir en paralelo tras A1.
