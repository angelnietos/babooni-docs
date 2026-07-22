<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F65-A3 — Next confirm + toast CRUD</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F65" src="https://img.shields.io/badge/round-F65-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Next sin Lit: `showToast` + `ArqConfirmDialog` en `@base/next-ui`; wire
clients/users (roles Next N/A).

## Criterios de aceptación

- [x] Delete pide confirm; toasts visibles.
- [x] Users delete vía `removeUser` data-access.

## Verificación

```bash
pnpm nx typecheck base-next-ui
pnpm nx typecheck base-next-clients-features
pnpm nx typecheck base-next-users-features
```
