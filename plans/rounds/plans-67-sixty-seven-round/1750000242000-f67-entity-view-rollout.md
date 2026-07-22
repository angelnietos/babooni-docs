<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F67-A3 — Entity view rollout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F67" src="https://img.shields.io/badge/round-F67-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Extender el piloto F66-A2 (`EntityFieldAccess` / `EntityFormViewBase` /
`useEntityView`) a **al menos un dominio adicional** y montar form/detail en
el **slot** del FeatureShell (no templates duplicados read vs write).

Contrato: [entity-view-abstractions.md](../../../frontend/entity-view-abstractions.md).

## Entregables

1. Config EntityView para **users** o **roles** (create/edit/detail mixed).
2. ≥2 stacks: Angular (+ Ionic o React) consumen la misma config shape.
3. Form/detail montado desde panel del shell (modal/drawer/route child) en
   ≥1 dominio — no page monolítica con campos hardcodeados.
4. Actualizar guía: matriz dominio × stack × modes.

## Criterios de aceptación

- [ ] ≥1 dominio nuevo con `EntityViewConfig` (no solo clients).
- [ ] Mixed read/write en ≥1 pantalla sin duplicar template por modo.
- [ ] Slot shell + entity view en ≥1 dominio (enlace A2 ↔ FeatureShell).
- [ ] Tests unitarios del helper/base (≥1 stack) o spec de config.

## Verificación

```bash
pnpm nx typecheck base-angular-users-features
# o roles / react equivalente según piloto
pnpm nx typecheck base-shared
```

## Depende de

F66-A2; F67-A2 facilita el slot del shell.
