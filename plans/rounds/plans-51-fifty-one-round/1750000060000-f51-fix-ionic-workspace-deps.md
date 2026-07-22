<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F51-A1 — Reparar deps workspace Ionic / `pnpm install`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Varios filtros `pnpm install` fallan con 404 npm de paquetes `@base/ionic-*`
que deberían resolverse como `workspace:*` (p. ej. `@base/ionic-dashboard-api`,
`@base/ionic-auth-data-access`). Sin esto no se puede actualizar lockfile ni
añadir paquetes nuevos de forma fiable.

## Resultado

**Causa:** las libs dashboard (Ionic + RN) declaraban deps internas como
`"0.0.0"` en lugar de `"workspace:*"`. pnpm interpretaba eso como versión npm
pública → 404.

**Fix (11 pins → `workspace:*`):**

| Package | Deps corregidas |
|---------|-----------------|
| `@base/ionic-dashboard-data-access` | `@base/ionic-dashboard-api` |
| `@base/ionic-dashboard-features` | auth-data-access, dashboard-data-access, ionic-ui |
| `@base/ionic-dashboard-shell` | auth-data-access, dashboard-features |
| `@base/react-native-dashboard-data-access` | dashboard-api |
| `@base/react-native-dashboard-features` | ui, dashboard-api, dashboard-data-access |
| `@base/react-native-dashboard-shell` | dashboard-features |

- `pnpm install` raíz → exit 0.
- Inventario: cero `@base|@arquetipos|@josanz|@saas` sin protocolo `workspace:` en deps.
- Nota en [local-development.md](../../../guides/local-development.md) (tabla troubleshooting).
- `check:workspace-deps:strict` sigue con violaciones **preexistentes** ajenas
  (undeclared imports en arquetipos/josanz/react); no introducidas por este fix.

## Criterios de aceptación

- [x] `pnpm install` en raíz exit 0.
- [x] Cero referencias a `@base/ionic-*` (y RN dashboard) tipadas como versión npm.
- [x] Nota en Resultado con lista de packages tocados.
