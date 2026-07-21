# Ronda F51 — Higiene workspace + cobertura + mobile layers + SaaS typecheck + npm publish

## Estado

en progreso

> A1/A2 completados; A3 mid-impl posible. B1/C*/E1/D1 pendientes.
> Siguiente ronda preparada: [F52](../plans-52-fifty-two-round/).

## Dependencias externas

- F50 completada (typecheck FE, tests BE, int-specs, Metro/CI docs).
- Carry: cobertura formal, phantom Ionic deps, dashboard mobile layers, SaaS CRM tsc.
- Política semver docs (F44) sin tooling de release/publish.

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F51-A1 | [Reparar deps workspace Ionic / pnpm install](1750000060000-f51-fix-ionic-workspace-deps.md) | Carry F50 / DX |
| F51-A2 | [Completar capas dashboard mobile (lib-layout)](1750000061000-f51-complete-mobile-dashboard-layers.md) | lib-layout warn |
| F51-A3 | [Cobertura backend audit/settings/tenants + gate](1750000062000-f51-backend-coverage-thresholds.md) | Carry F50-A2 |
| F51-B1 | [Typecheck libs SaaS CRM (más allá de la app)](1750000063000-f51-saas-crm-libs-typecheck.md) | Carry F50-A1 |
| F51-C1 | [Paquete `@arquetipos/expo-metro-config` (opcional)](1750000064000-f51-expo-metro-workspace-package.md) | DX Metro |
| F51-C2 | [Higiene artefactos + ignore tsbuildinfo](1750000065000-f51-hygiene-tsbuildinfo-and-artifacts.md) | Calidad |
| F51-E1 | [Publicación npm, `.npmrc` y versionado de libs](1750000067000-f51-npm-publish-and-lib-versioning.md) | Producto / DX |
| F51-D1 | [Documentación y cierre F51](1750000066000-f51-documentation.md) | Cierre |

## Objetivo

1. Desbloquear `pnpm install` / lockfile ante deps Ionic fantasma.
2. Cerrar warns de lib-layout en dashboard mobile (ionic + rn).
3. Umbral de cobertura medible en dominios audit/settings/tenants.
4. Typecheck de libs `@saas/*` CRM alineado con la app canario.
5. (Opcional) Metro como paquete workspace sin allowlist ESLint.
6. No commitear `*.tsbuildinfo`; higiene de artefactos.
7. **Sistema de publicación npm + versionado semver** para libs UI/dominio/base
   (`.npmrc`, Nx Release, deps/peers, guía).

## Orden de ejecución

1. F51-A1 (pnpm) — bloquea lockfile y **E1** (release/publish).
2. F51-A2 (mobile layers) en paralelo a A3.
3. F51-B1 (SaaS typecheck).
4. F51-E1 (npm publish / versionado) — tras A1; puede ir en paralelo a B1/C*.
5. F51-C1 / C2 opcionales.
6. F51-D1 cierre.

## Riesgos

- A1 puede requerir crear stubs/packages o corregir `workspace:*` rotos.
- Coverage thresholds: no bajar la barra si el harness local no mide igual que CI.
- E1: publicar con `workspace:*` sin reescritura; peers Angular/React mal puestos;
  scopes producto en registry sin acuerdo.

## Criterios de aceptación

- [x] `pnpm install` (root) no falla por `@base/ionic-*` 404.
- [x] `check-lib-layout` sin warn de dashboard mobile incompleto.
- [x] Cobertura audit/settings/tenants documentada + script/gate.
- [x] Typecheck verde en libs CRM objetivo (lista B1).
- [ ] Oleada canario npm: `.npmrc.example` + dry-run pack/publish + guía.
- [ ] Hub + plans: F51 activa o cerrada; F50 en archivo; **F52** enlazada como siguiente.
- [ ] `pnpm check:deprecated` + `pnpm check:exports-paths` pasan.

## Siguiente

Tras F51-D1 → [plans-52-fifty-two-round](../plans-52-fifty-two-round/)
(carry publish, workspace-deps strict, storybook, coverage CI soft, peers, docs).
