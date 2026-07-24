<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F79-A1 — Atom matrix & migration contract</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F79" src="https://img.shields.io/badge/round-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24

## Objetivo

Definir la **matriz canónica** átomo Lit → adapter por framework y las reglas de migración, para que B1–D1 no improvisen wrappers ni dejen stacks incompletos.

## Motivación

Hoy Next/Ionic mantienen forks visuales; RN no puede hospedar Lit. Sin contrato explícito:

- se “documenta menos” en Storybook de adapters;
- Arquetipos diverge por runtime;
- se mezclan chrome de plataforma (`ion-tabs`) con átomos de diseño (`button`).

## Contrato (reglas)

| Clase | Definición | Next | Ionic | RN |
|-------|------------|------|-------|-----|
| **SoT atom** | CE en `@base/native-ui` | Wrapper `'use client'` / CE island (reutilizar `@base/react-ui` `Native*` cuando sea posible) | Thin wrapper Angular CE **o** CE en template; no `ion-button` como SoT visual | Twin JSX + tokens (`@base/ui-tokens/rn`) — misma API conceptual |
| **Platform chrome** | Navegación, safe-area, keyboard, tab bar nativa | Layout Next / app shell | `ion-*` permitido | RN Navigation / native chrome |
| **Composite** | Feature shell, page header, table toolbar | Puede componer SoT atoms; no reinventar button/input | Igual | Igual |
| **Brand** | Product/arquetipos UI | Re-export o wrap fino sobre adapter base | Igual | Igual |

### Decisiones fijas

1. **Nuevos átomos** → siempre primero en `base-native-ui` (+ story Lit) → luego oleada adapters.
2. **No** añadir variantes visuales solo en Next/Ionic CSS forks.
3. **Paridad Storybook** = mismo set de SoT atoms documentado; chrome-only stories son extra, no sustituyen.
4. **Deprecation**: forks CSS/`ion-*` como átomo de diseño → `@deprecated` + alias al wrapper Lit durante 1 ronda; delete en F80 si estable.

## Inventario inicial (rellenar en ejecución)

Partir del catálogo Lit actual (~23):

`alert`, `avatar`, `badge`, `board`, `button`, `card`, `checkbox`, `chip`, `divider`, `empty-state`, `icon`, `input`, `modal`, `pagination`, `progress`, `radio`, `segment`, `select`, `skeleton`, `spinner`, `tabs`, `toast`, `toggle`.

Para cada uno, tabla en `docs/frontend/native-ui-adapter-matrix.md` (crear):

| Atom | Lit tag | Angular | React | Next | Ionic | RN | Notes |
|------|---------|---------|-------|------|-------|----|-------|

## Acciones

- [ ] Auditar exports de `base-native-ui`, `base-react-ui` (`Native*`), `base-angular-ui`, `base-next-ui`, `base-ionic-ui`, `base-react-native-ui`
- [ ] Clasificar cada export: SoT atom / chrome / composite / brand
- [ ] Publicar `docs/frontend/native-ui-adapter-matrix.md`
- [ ] Enlazar desde [design-system.md](../../../frontend/design-system.md) y ADR 0010/0011 “See also”
- [ ] Definir orden de oleadas (sugerido: button → input → select → feedback → layout atoms → board)

## Criterios de aceptación

- [ ] Matriz publicada y completa para el set Lit actual
- [ ] Reglas wrap/chrome/twin aceptadas en README F79
- [ ] Ningún plan B* empieza sin fila de matriz para el átomo tocado

## Verificación

```bash
# doc-only + grep inventory
rg "export (function|const|class)" libs/base/frontend/crosscutting/native-ui/src -g '!*.stories.*'
rg "export .*Arq" libs/base/frontend/next/ui/src libs/base/frontend/mobile/ionic/ui/src libs/base/frontend/mobile/rn/ui/src
```
