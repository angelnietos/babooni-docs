<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F65-A1 — SPA Angular/React confirm+toast closeout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F65" src="https://img.shields.io/badge/round-F65-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Cerrar y verificar el patrón ya iniciado en plantillas SPA: riesgo →
`ArqConfirmDialog`; mutaciones → `showToast` (`@base/native-ui`).

## Entregables

1. Clients / users / roles Angular + React con confirm delete + toast create/save/delete.
2. Export estable `ArqConfirmDialog` en `@arquetipos/arquetipos-{angular,react}-ui`.
3. Typecheck verde en features tocadas.

## Criterios de aceptación

- [x] Delete no muta hasta confirmar.
- [x] Toasts success/warning/danger en caminos felices/error.
- [x] Roles: toast en create/save (sin delete → N/A).

## Verificación

```bash
pnpm nx typecheck arquetipos-angular-clients-features
pnpm nx typecheck arquetipos-react-clients-features
pnpm nx typecheck arquetipos-angular-users-features
pnpm nx typecheck arquetipos-react-users-features
pnpm nx typecheck arquetipos-angular-roles-features
pnpm nx typecheck arquetipos-react-roles-features
```

## Depende de

— (base native-ui toast + modal).
