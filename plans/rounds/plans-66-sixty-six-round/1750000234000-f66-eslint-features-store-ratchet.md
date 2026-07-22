<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F66-B3 — ESLint features ↔ store ratchet</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F66" src="https://img.shields.io/badge/round-F66-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Cerrar el defer de F65-D1: impedir que `*-features` importen stores concretos
(`@ngrx/store` `Store`, `useAppDispatch`, `*Store` de list CRUD) cuando el
dominio ya tiene facade (piloto **clients**; opcional users).

## Entregables

1. Regla ESLint o check en `tools/checks/` (warn → strict).
2. Allowlist temporal documentada si hace falta.
3. CI soft → fail cuando clients features estén limpios.

## Criterios de aceptación

- [x] Check existe y corre en hygiene o job `frontend`.
- [x] Clients features (Angular + React) pasan en strict **o** defer F67 con
      lista de excepciones.

## Verificación

```bash
# nombre final del script a definir en la tarjeta
pnpm hygiene
# o node tools/checks/check-features-no-store.mjs --strict
```

## Depende de

F65-D1; F66-A1 facilita el cumplimiento.
