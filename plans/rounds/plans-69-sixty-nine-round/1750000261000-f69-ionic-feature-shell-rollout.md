<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F69-A2 — Ionic FeatureShell rollout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F69" src="https://img.shields.io/badge/round-F69-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Extender `ArqIonFeatureShell` (F68-A3 piloto clients) a **users / roles / audit**
(u ≥2 de ellos), eliminando chrome `arq-feature*` duplicado en templates.

## Entregables

1. Refactor ≥2 páginas Ionic list → `arq-ion-feature-shell`.
2. Specs actualizados (mock shell).
3. Matriz stack × adopción en feature-shell-presentation.md.
4. Sin `MobileXPage` / `DesktopXPage`.

## Criterios de aceptación

- [ ] ≥2 dominios Ionic montan thin wrapper **o** defer F70 con motivo.
- [ ] Typecheck + test features afectados en verde.

## Verificación

```bash
pnpm nx typecheck base-ionic-users-features
pnpm nx typecheck base-ionic-roles-features
pnpm nx test base-ionic-users-features
```
