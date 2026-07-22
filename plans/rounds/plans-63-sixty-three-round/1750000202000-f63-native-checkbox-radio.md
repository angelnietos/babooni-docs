<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F63-B1 — Native checkbox / radio atoms</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Sustituir checkboxes/radios OS en roles (permisos) y settings por átomos
`@base/native-ui` alineados a tokens (marca, focus, disabled).

## Entregables

1. `base-checkbox` / `base-radio` (o ampliar existentes) en native-ui.
2. Adoptar en roles permissions grid + theme pickers que aún usen input nativo.
3. Story mínimo + a11y (label, keyboard, `aria-checked`).

## Criterios de aceptación

- [ ] Camino feliz roles sin `<input type="checkbox">` OS visible.
- [ ] Focus ring usa `--focus-ring` / brand tenant.

## Verificación

`pnpm nx test base-native-ui` + manual `/roles`.

## Depende de

F62-C2 (hecho).
