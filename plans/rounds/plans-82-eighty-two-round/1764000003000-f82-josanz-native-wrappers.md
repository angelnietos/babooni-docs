<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F82-C1 — Wrappers Josanz sobre Native*</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F82" src="https://img.shields.io/badge/round-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

**hecho** · 2026-07-24 · depende de [F82-A1](1764000001000-f82-josanz-native-inventory.md) (+ [B1](1764000002000-f82-extend-base-native.md))

## Objetivo

Que la app Angular Josanz **se vea igual que ahora**, pero los controles WRAP usen Lit SoT vía `@base/angular-ui` `Native*` + wrapper de marca en `@josanz/angular-ui` (tema, defaults, selectores `josanz-*` estables).

## Wrappers ejecutados

| Componente | Selector | Adapter |
|------------|----------|---------|
| Button | `josanz-button` | `NativeButtonComponent` |
| Input | `josanz-input` | `NativeInputComponent` |
| Alert | `josanz-alert` | `NativeAlertComponent` |
| Checkbox | `josanz-checkbox` | `NativeCheckboxComponent` |
| Spinner | `josanz-spinner` | `NativeSpinnerComponent` |
| Badge | `josanz-badge` | `NativeBadgeComponent` |

Host: clase `josanz-native` + mapa CSS vars en `theme/josanz-native-host-vars.ts`.

**Deferred:** Select, Modal (sheet gap — sigue implementación Josanz), Toast/Toggle oleada completa.

## Acciones

- [x] Canarios: Button, Input, Alert (mismos selectors / exports)
- [x] Oleada parcial: Badge, Checkbox, Spinner
- [x] RE-EXPORT donde no haya brand (Topbar / Base* aliases intactos)
- [x] JOSANZ-ONLY sin tocar (catalog, settings, delete, Figma shells)
- [x] `registerNativeUi` en app Josanz `main.ts` + Storybook preview
- [x] Tests canario `josanz-angular-ui` verdes; typecheck lib OK
- [x] Features siguen importando `@josanz/angular-ui` (ownership)

## Criterios

- [x] Barrel `@josanz/angular-ui` no rompe named exports
- [x] Paridad visual vía tokens `--josanz-*` → `--color-*` / `--status-*` (checklist E1)
- [x] Guía wrap vs re-export respetada
