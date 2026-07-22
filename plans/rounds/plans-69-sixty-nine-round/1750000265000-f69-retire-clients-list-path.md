<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F69-B3 — Retire clients-list path alias</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F69" src="https://img.shields.io/badge/round-F69-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

F68-A2 retiró dual selectors; quedan thin re-exports `clients-list.css` /
package exports / tsconfig paths. Eliminar cuando inventario path=0.

## Entregables

1. Inventario `clients-list` path consumers.
2. Si 0: borrar alias files + exports + paths; actualizar tests.
3. Si residual: lista + defer F70.

## Criterios de aceptación

- [ ] Alias path retirado **o** inventario + defer F70.
- [ ] `pnpm nx test base-ui-styles` verde.

## Verificación

```bash
rg -n "clients-list" libs apps tsconfig.base.json --glob "!**/node_modules/**" || true
pnpm nx test base-ui-styles
```
