<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F82-B1 — Extender base native (gaps reutilizables)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F82" src="https://img.shields.io/badge/round-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

**hecho** · 2026-07-24 · depende de [F82-A1](1764000001000-f82-josanz-native-inventory.md)

## Objetivo

Donde el inventario marque **PROMOTE-TO-BASE**, enriquecer `@base/native-ui` (y adapters `@base/angular-ui` `Native*`) para que Josanz pueda wrappear sin reinventar CSS dual.

## Promotes ejecutados (trazables a A1)

| Gap A1 | Cambio SoT / adapter |
|--------|----------------------|
| Button `outline` + `xl` | `NativeButtonVariant` / `NativeButtonSize` + estilos Lit |
| Alert `dismissible` + tone `neutral` | `base-alert` + `NativeAlertComponent` (`closed`) |
| Checkbox `description` | `base-checkbox` + `NativeCheckboxComponent` |
| Badge tone `neutral` | `TONE_VARS` en `base-badge` |

**Defer (no bloquean canarios):** `base-textarea`, modal sheet, overlay primitives, date/autocomplete/…

## Acciones

- [x] Solo gaps con evidencia de reuso — no subir brand Josanz a base
- [x] Implementar/ajustar Lit CE + tests SoT (`base-native-ui`)
- [x] Actualizar adapters Angular `Native*`
- [x] `pnpm nx test base-native-ui` verde
- [x] Matriz/inventario documenta API nueva ([josanz-native-inventory.md](../../../frontend/josanz-native-inventory.md))

## Criterios

- [x] Cada promote trazable a fila A1
- [x] SoT tests verdes para átomos tocados
- [x] Cero tokens `--josanz-*` en `@base/native-ui`
