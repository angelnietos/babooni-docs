<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F82-C1 — Wrappers Josanz sobre Native*</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F82" src="https://img.shields.io/badge/round-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · 2026-07-24 · depende de [F82-A1](1764000001000-f82-josanz-native-inventory.md) (+ [B1](1764000002000-f82-extend-base-native.md) si hay gaps)

## Objetivo

Que la app Angular Josanz **se vea igual que ahora**, pero los controles WRAP usen Lit SoT vía `@base/angular-ui` `Native*` + wrapper de marca en `@josanz/angular-ui` (tema, defaults, selectores `josanz-*` estables).

## Acciones

- [ ] Canarios: Button, Input, Alert (mismo selector / exports públicos del barrel)
- [ ] Oleada controles: Select, Modal, Badge, Checkbox, Toggle, Spinner, Toast, …
- [ ] RE-EXPORT donde no haya brand (YAGNI)
- [ ] JOSANZ-ONLY sin tocar (sigue CSS/Angular propio)
- [ ] `registerNativeUi` en bootstrap app / Storybook Josanz si aún no
- [ ] Smoke visual: `apps/clientes/josanz` + Storybook `josanz-angular-ui`
- [ ] Features `@josanz/*-features` siguen importando `@josanz/angular-ui` (no `@base/angular-ui` directo — ownership)

## Criterios

- [ ] Barrel `@josanz/angular-ui` no rompe named exports usados por features/apps
- [ ] Paridad visual aceptada en canarios (checklist en E1)
- [ ] Guía wrap vs re-export respetada (no wrappers vacíos)
