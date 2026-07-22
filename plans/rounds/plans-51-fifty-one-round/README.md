<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F51 — Higiene workspace + cobertura + mobile layers + SaaS typecheck + npm publish</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Dependencias externas

- F50 completada (typecheck FE, tests BE, int-specs, Metro/CI docs).
- Carry resuelto aquí; resto publish real / CI coverage → F52.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F51-A1 | [Reparar deps workspace Ionic / pnpm install](1750000060000-f51-fix-ionic-workspace-deps.md) | completado |
| F51-A2 | [Completar capas dashboard mobile (lib-layout)](1750000061000-f51-complete-mobile-dashboard-layers.md) | completado |
| F51-A3 | [Cobertura backend audit/settings/tenants + gate](1750000062000-f51-backend-coverage-thresholds.md) | completado |
| F51-B1 | [Typecheck libs SaaS CRM (más allá de la app)](1750000063000-f51-saas-crm-libs-typecheck.md) | completado |
| F51-C1 | [Paquete `@arquetipos/expo-metro-config`](1750000064000-f51-expo-metro-workspace-package.md) | completado |
| F51-C2 | [Higiene artefactos + ignore tsbuildinfo](1750000065000-f51-hygiene-tsbuildinfo-and-artifacts.md) | completado |
| F51-E1 | [Publicación npm, `.npmrc` y versionado de libs](1750000067000-f51-npm-publish-and-lib-versioning.md) | completado |
| F51-D1 | [Documentación y cierre F51](1750000066000-f51-documentation.md) | completado |

## Criterios de aceptación

- [x] `pnpm install` (root) no falla por `@base/ionic-*` 404.
- [x] `check-lib-layout` sin warn de dashboard mobile incompleto.
- [x] Cobertura audit/settings/tenants documentada + script/gate.
- [x] Typecheck verde en libs CRM objetivo (lista B1).
- [x] Oleada canario npm: `.npmrc.example` + dry-run pack + guía.
- [x] Hub + plans: F51 archivo; **F52** activa.
- [x] `pnpm check:deprecated` + `pnpm check:exports-paths` pasan.

## Siguiente

→ [plans-52-fifty-two-round](../plans-52-fifty-two-round/)
(carry publish real, workspace-deps strict, storybook, coverage CI soft, peers, docs).
