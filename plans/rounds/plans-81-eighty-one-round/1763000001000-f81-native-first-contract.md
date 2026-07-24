<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F81-A1 — Native-first contract (Arquetipos × frameworks)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F81" src="https://img.shields.io/badge/round-F81-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24

## Objetivo

Fijar que **todas** las plantillas Arquetipos consumen el **mismo set de átomos SoT** (`@base/native-ui` vía adapter), con un path canónico por runtime y un gate que detecte regresiones a primitivos CSS/legacy.

## Path canónico por runtime

| Runtime | Package features | Átomos SoT |
|---------|------------------|------------|
| Angular | `@arquetipos/arquetipos-angular-ui` | `ArqNative*Component` (→ `@base/angular-ui` `Native*`) |
| React | `@arquetipos/arquetipos-react-ui` | `ArqNative*` (→ `@base/react-ui` `Native*`) |
| Next | `@arquetipos/arquetipos-next-ui` / `@base/next-ui` | `Arq*` client islands → Lit |
| Ionic | `@arquetipos/arquetipos-ionic-ui` / `@base/ionic-ui` | `ArqIon*` Lit + chrome `ion-*` |
| RN | `@arquetipos/arquetipos-react-native-ui` / `@base/react-native-ui` | `Arq*` twin tokens (sin Lit) |

Chrome de plataforma (shell, tabs Ion, Topbar) **no** cuenta como fork de átomo SoT.

## Acciones

- [x] Actualizar [arquetipos-thin-libs.md](../../../frontend/arquetipos-thin-libs.md) + [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md): adapters viven en `atoms/{name}/`, no en `wrappers/native/`
- [x] Extender `tools/checks/check-ui-native-first.mjs` (o hermano) a:
  - `libs/arquetipos/frontend/{angular,react,next,mobile/**}/**/features/`
  - símbolos legacy: `ButtonComponent`, `ArqButtonComponent`, `ArqButton` (no Native), `InputComponent`, etc.
- [x] Soft en CI → strict al cerrar F81-C1/D1 (allowlist documentada; hygiene ya usa `--strict` + ratchet)
- [x] Matriz dominio × átomo mínimo (auth/clients): Button, Input, Alert, EmptyState, Spinner, Select

## Criterios

- [x] Doc de contrato enlazada desde design-system
- [x] Gate soft no-cero allowlist documentada; plan de ratchet
