<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F59-A3 — Adapters Angular/React + error envelope FE</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F60-E1 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Hacer reutilizable el kit A1/A2: helpers genéricos para cablear reglas shared a
Reactive Forms (Angular) y a forms React (controlled / RHF si ya existe en
repo; si no, helper mínimo sin nueva deps).

## Entregables

1. `@base/angular` o `@base/angular-ui` / data-access: `rulesToValidators(rules)`
   (o ubicación acordada en A1 — preferir package cercano a forms sin romper
   layering).
2. `@base/react` / react-shared: equivalente (`rulesToFieldErrors` /
   `validateValues`).
3. Mapper de errores Nest (`message` / `ValidationError[]`) → field errors FE
   (reusar/extender `http-api-error-message` SaaS si aplica; subir a base).
4. Story o ejemplo mínimo en docs (no Storybook obligatorio).

## Criterios de aceptación

- [ ] Adapters documentados y tipados; usados por el piloto A2.
- [ ] Un test unit por adapter.
- [ ] Layering intacto (`check:lib-layout`, ownership UI).

## Depende de

F59-A1 (API del kit); idealmente en paralelo / justo después de A2.
