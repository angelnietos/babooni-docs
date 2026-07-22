<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F59 — Validaciones isomórficas + carries F58</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado (con carries → F60)

> Cerrada 2026-07-22. Único hito ejecutado: **C1** (e2e/smoke Next·Ionic·RN).
> Validación isomórfica (A*), visual tooling (B*), Problem+JSON (D1) y polish
> docs (E1) → **[F60](../plans-60-sixty-round/)**.

## Contexto

Hoy el backend valida con `class-validator` + `ValidationPipe` sobre DTOs en
`@base/shared` / producto. El frontend (Angular `Validators.*`, formularios
Josanz, React) **repite** reglas a mano sin enlace al DTO. F59 abrió el trabajo;
la ejecución del kit isomórfico se traslada a F60-E1 junto a la ronda de
native-ui / Storybook.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F59-A1 | [Estrategia validación isomórfica (ADR + kit)](1750000160000-f59-isomorphic-validation-strategy.md) | trasladado → F60-E1 |
| F59-A2 | [Piloto clients: reglas compartidas FE+BE](1750000161000-f59-validation-pilot-clients.md) | trasladado → F60-E1 |
| F59-A3 | [Adapters Angular/React + error envelope](1750000162000-f59-validation-fe-adapters.md) | trasladado → F60-E1 |
| F59-A4 | [Scaffolds + gate drift validación](1750000163000-f59-validation-scaffolds-gate.md) | trasladado → F60-E1 |
| F59-B1 | [Chromatic o alternativa visual](1750000164000-f59-chromatic-or-visual-alt.md) | trasladado → F60-E2 |
| F59-B2 | [Figma Code Connect pilot](1750000165000-f59-figma-code-connect-pilot.md) | trasladado → F60-E2 |
| F59-B3 | [Remove framework `@deprecated` primitives](1750000166000-f59-remove-deprecated-primitives.md) | trasladado → F60-E2 |
| F59-C1 | [E2E / smoke Next · Ionic · RN](1750000167000-f59-e2e-next-ionic-rn.md) | completado |
| F59-D1 | [Problem+JSON / códigos error validación unificados](1750000168000-f59-validation-error-codes.md) | trasladado → F60-E1 |
| F59-E1 | [Docs polish + hub F59](1750000169000-f59-documentation-polish.md) | trasladado → F60-F1 |

## Criterios de aceptación (ronda)

- [x] Next/Ionic/RN: smoke Playwright + scripts (`arq:fe:e2e:next`, `arq:mobile:e2e:smoke`).
- [x] Carry explícito del resto → F60 (no silencio).
- [x] Hub plans + biblia apuntan a F59 cerrada / F60 activa.

## Fuera de alcance (histórico)

- Reescribir todos los dominios de golpe.
- Sustituir Nest `ValidationPipe` global sin migración gradual.
