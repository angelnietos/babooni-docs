<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F68-A2 — Retire arq-clients* alias / dual CSS</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F68" src="https://img.shields.io/badge/round-F68-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Carry residual de [F67-A2](../plans-67-sixty-seven-round/1750000241000-f67-feature-shell-adoption-closeout.md):
retirar alias `arq-clients*` / `clients-list.css` y dual-CSS cuando el
inventario de consumers llegue a **cero**.

SoT: `@base/ui-styles` → `pages/feature-list` (`arq-feature*`).

## Entregables

1. Inventario actualizado de consumers `arq-clients*` / `clients-list`.
2. Si consumers=0:
   - eliminar dual selectors y alias de export;
   - smoke `pnpm nx test base-ui-styles` + build Angular arquetipos.
3. Si quedan consumers: lista + owner + defer F69 (no silenciar).
4. Actualizar [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md)
   + [ui-styles-7-1.md](../../../frontend/ui-styles-7-1.md).

## Criterios de aceptación

- [x] Alias retirado **o** inventario residual + defer F69 con owner.
- [x] Sin regenerar dual-CSS naive (`@charset` / `@keyframes` / `var()`).
- [x] Build Angular arquetipos sin warnings CSS del alias.

## Verificación

```bash
pnpm nx test base-ui-styles
rg -n "arq-clients" libs apps --glob "!**/node_modules/**" || true
```

## Depende de

F67-A2 (migración parcial hecha; alias conservado).
