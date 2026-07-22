<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F59-A4 — Scaffolds + gate drift validación</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F60-E1 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Que dominios nuevos no reintroduzcan drift: scaffolds emiten stub de reglas
shared + DTO + recordatorio FE; check opcional detecta dominios con DTO
class-validator sin reglas shared (soft → strict selectivo).

## Entregables

1. `gen-domain` / scaffold arquetipos: archivo
   `…/shared/…/validation/<domain>.rules.ts` (o convención A1) + enlace en DTO.
2. Post-scaffold checklist: “cablear form FE a shared rules”.
3. Script `tools/checks/check-validation-parity.mjs` (soft): lista dominios con
   `Create*Dto` sin módulo rules companion (allowlist inicial).
4. CI soft opcional en `quality` o solo local documentado.

## Criterios de aceptación

- [ ] Dry-run scaffold muestra archivo de rules.
- [ ] Check soft documentado en `ci-gates.md`.
- [ ] No fail PR masivo el día 1 (allowlist / soft).

## Depende de

F59-A1 + A2 (convención estable).
