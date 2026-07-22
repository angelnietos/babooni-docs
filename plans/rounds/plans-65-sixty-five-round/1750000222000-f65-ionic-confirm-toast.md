<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F65-A2 — Ionic confirm + toast CRUD</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F65" src="https://img.shields.io/badge/round-F65-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Paridad confirm/toast en Ionic. Thin arquetipos → `@base/ionic-*` + helpers
en `@base/ionic-ui` (`confirmArqIonDanger`, `showArqIonToast`).

## Criterios de aceptación

- [x] Trash abre alert destructivo; cancel no muta.
- [x] Toast success tras delete/save; warning si form inválido.

## Verificación

```bash
pnpm nx typecheck base-ionic-clients-features
pnpm nx typecheck base-ionic-users-features
pnpm nx typecheck base-ionic-roles-features
```
