<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-A1 — Select portal verify + regression</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Cerrar [F63-A1](../plans-63-sixty-three-round/1750000200000-f63-select-portal-hardening.md):
portal a `document.body` ya existe; faltan ACs manuales, tests y story canario.

## Entregables

1. Smoke manual `angular-multi` / `react-multi`: Tenant listbox visible bajo shell
   con `backdrop-filter`; 5 orgs; Escape / click-outside / reposition scroll.
2. Tests `base-native-ui`: portal + `base-change` + no close en click de apertura.
3. Story Storybook: `base-select` abierto con portal (alimenta C1).
4. Nota corta en `design-system.md` si aún falta (overlays → portal).

## Criterios de aceptación

- [ ] En `/clients` multi, abrir Tenant muestra nova/helix/orbit/meridian/lumen.
- [ ] Panel no clipado por stage/topbar.
- [ ] `pnpm nx test base-native-ui` verde en select/portal.

## Verificación

```bash
pnpm nx test base-native-ui
# Manual: angular-multi :4203 — Tenant switcher
```

## Depende de

—

## Bloquea

F64-A2 (patrón), F64-C1 (story select).
