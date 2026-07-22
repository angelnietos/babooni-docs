<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F70-A2 — Ionic FeatureShell rollout: audit</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F70" src="https://img.shields.io/badge/round-F70-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Extender `ArqIonFeatureShell` (F68-A3 piloto clients; F69-A2 users/roles) a
**audit**, eliminando chrome `arq-feature*` duplicado en templates.

## Entregables

1. Refactor audit page → `arq-ion-feature-shell`.
2. Specs actualizados (mock shell).
3. Matriz stack × adopción en feature-shell-presentation.md.
4. Sin `MobileXPage` / `DesktopXPage`.

## Criterios de aceptación

- [ ] Audit monta `ArqIonFeatureShell` con `presentation="auto"`.
- [ ] Typecheck + test features afectados en verde.

## Verificación

```bash
pnpm nx typecheck base-ionic-audit-features
pnpm nx typecheck base-ionic-audit-shell
```
