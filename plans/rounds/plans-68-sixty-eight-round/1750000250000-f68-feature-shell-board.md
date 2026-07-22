<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F68-A1 — Native board CE + FeatureShell board</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F68" src="https://img.shields.io/badge/round-F68-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Carry de [F67-A1](../plans-67-sixty-seven-round/1750000240000-f67-feature-shell-board.md):
entregar **Native board CE** (Lit) + wrappers Angular/React, y cablear
`presentation: 'board'` en FeatureShell.

Contrato: [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md).

## Blocker (F67)

No base-board CE. Owner: design-system / FE platform.

## Entregables

1. CE `<base-board>` (o equivalente) en `@base/native-ui` + story a11y.
2. Wrappers `NativeBoard` Angular / React.
3. Strategy `board` en `FeatureShellComponent` + `FeatureShell`.
4. Piloto ≥1 dominio arquetipos o story de shell.
5. Actualizar guía (matrix presentation × breakpoint).

## Criterios de aceptación

- [x] Decisión explícita: implementado **o** defer F69 (owner + blocker).
- [x] Si implementado: `presentation: 'board'` tipado; smoke visual ≥1 stack.
- [x] Guía `feature-shell-presentation.md` refleja el estado real.

## Verificación

```bash
pnpm nx typecheck base-native-ui
pnpm nx typecheck base-angular-ui
pnpm nx typecheck base-react-shared
# visual: story / piloto board
```

## Depende de

F67-A1 (defer). Design-system capacity para CE board.
