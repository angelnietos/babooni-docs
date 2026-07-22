<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-D1 — Carry: validación isomórfica</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Retomar validación compartida FE↔BE deferred F59→F60→F62→F63
([F63-D1](../plans-63-sixty-three-round/1750000205000-f63-carry-isomorphic-validation.md)).

## Entregables

1. Piloto **clients** Create/Update: reglas en `@base/shared` (o kit acordado).
2. Adapters Angular + React (y nota Ionic/Next).
3. Gate opcional `tools/checks/check-validation-parity.mjs` **o** tests de paridad.
4. Si no cabe en la ronda: defer F65 con **motivo + owner** en hub (prohibido
   silencio).

## Criterios de aceptación

- [ ] Piloto clients verde **o** defer F65 explícito.
- [ ] Docs: enlace desde guides / ADR si el kit nace.

## Verificación

Criterios del plan F60-E1 / F59-A* referenciados en carries previos.

## Depende de

—
