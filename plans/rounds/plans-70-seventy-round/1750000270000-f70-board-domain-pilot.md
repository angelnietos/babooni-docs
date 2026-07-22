<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F70-A1 — Board domain pilot</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F70" src="https://img.shields.io/badge/round-F70-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

F69-A3 deferió board piloto por falta de owner + dominio canario.
F70-A1 entrega **≥1 página arquetipos** con `presentation="board"` + NativeBoard
lanes más allá del Storybook.

## Entregables

1. Página/piloto con `presentation="board"` + NativeBoard lanes (p. ej. tasks
   demo, native-ui showcase, o roles pipeline).
2. Smoke visual / story FeatureShell+board.
3. Guía actualizada.

## Criterios de aceptación

- [ ] Piloto dominio **o** defer F71 (owner design-system / FE platform + blocker).
- [ ] Typecheck stacks tocados.

## Verificación

```bash
pnpm nx typecheck base-native-ui
pnpm nx typecheck base-angular-ui
```
