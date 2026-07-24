<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F79-B3 — React Native token / API parity</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F79" src="https://img.shields.io/badge/round-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24 · Depende de: [F79-A1](1761000001000-f79-atom-matrix-and-migration-contract.md)

## Objetivo

Alinear `@base/react-native-ui` con el **contrato de API y tokens** del Lit SoT (ADR 0010 §6), sin introducir Lit en el bundle RN.

## Paths

- Lib: `libs/base/frontend/mobile/rn/ui/` (`base-react-native-ui`, `@base/react-native-ui`)
- Tokens: `@base/ui-tokens` / `@base/ui-tokens/rn` (verificar path real en workspace)
- Storybook: react-native-web alias (`.storybook` ya scaffold)

## Problema

RN ya tiene twins (`ArqButton`, `ArqInput`, …) pero props/variantes/spacing pueden derivar del look Ionic/ad-hoc. Sin checklist vs Lit, Arquetipos RN “se parece” pero no es el mismo sistema.

## Estrategia

1. Por cada SoT atom de la matriz, documentar:
   - props canónicas (variant, size, disabled, …);
   - mapping token (color, radius, space, type);
   - **gap explícito** si la primitiva nativa no puede igualar (p.ej. board complejo).
2. Implementar/ajustar twins hasta cubrir el set SoT razonable en móvil (excluir board si out-of-scope documentado).
3. Storybook RN-web muestra las mismas stories semánticas (Default / Variants / Disabled).
4. Prohibido: `import` de `@base/native-ui` / `lit` desde código RN runtime.

## Acciones

- [ ] Diff props: Lit CE attributes vs `Arq*` RN
- [ ] Sincronizar tokens RN con web (`--arq-*` ↔ StyleSheet theme)
- [ ] Completar twins faltantes del set SoT (prioridad forms + feedback)
- [ ] Añadir test unitario ligero de theme/token keys si no existe
- [ ] Stories RN (F79-C1) por átomo alineado

## Criterios de aceptación

- [ ] Matriz columna RN completa (OK / gap / N/A)
- [ ] Ningún import Lit en `base-react-native-ui` / apps RN
- [ ] `pnpm nx typecheck base-react-native-ui` verde
- [ ] Storybook :4408 cubre el set acordado

## Verificación

```bash
pnpm nx typecheck base-react-native-ui
rg "@base/native-ui|from 'lit'|from \"lit\"" libs/base/frontend/mobile/rn
pnpm storybook:base-rn
```
