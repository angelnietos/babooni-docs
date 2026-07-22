<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F71-A2 — Expand board pilot</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F71" src="https://img.shields.io/badge/round-F71-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

F70-A1 entregó `/tasks` con `<base-board>`. F71-A2 expande el piloto a
**≥1 dominio adicional** y agrega e2e smoke desktop + narrow.

| Sub-ítem | Blocker |
|----------|---------|
| Board en roles (Kanban RBAC pipeline) | Sin datos demo reales; superficie chica |
| E2e smoke board (desktop + narrow) | Falta harness e2e Ionic multi |

## Entregables

1. `/roles` o `/clients` con `presentation="board"` + NativeBoard lanes.
2. E2e smoke: `ionic-multi-e2e` agrega assertions board desktop + narrow.
3. Guía actualizada con ejemplo de dominio real.

## Criterios de aceptación

- [ ] ≥2 dominios usan `presentation="board"` **o** defer F72 (owner + blocker).
- [ ] e2e board pasa en desktop y narrow.
- [ ] Typecheck stacks tocados.

## Verificación

```bash
pnpm nx typecheck base-native-ui
pnpm nx typecheck base-angular-ui
pnpm e2e:ionic-multi
```
