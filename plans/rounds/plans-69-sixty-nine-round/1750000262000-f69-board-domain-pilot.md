<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F69-A3 — Board domain pilot</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F69" src="https://img.shields.io/badge/round-F69-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

F68-A1 entregó `<base-board>` + wrappers + `presentation: 'board'`. Falta
**piloto de dominio** (≥1 página arquetipos o showcase FeatureShell board)
más allá del Storybook.

## Entregables

1. Página/piloto con `presentation="board"` + NativeBoard lanes (p. ej. tasks
   demo, native-ui showcase, o roles pipeline).
2. Smoke visual / story FeatureShell+board.
3. Guía actualizada.

## Criterios de aceptación

- [ ] Piloto dominio **o** defer F70 (owner design-system / FE platform).
- [ ] Typecheck stacks tocados.

## Verificación

```bash
pnpm nx typecheck base-native-ui
pnpm nx typecheck base-angular-ui
```
