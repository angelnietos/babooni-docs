<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F45-A3 — Expandir typecheck a todos los proyectos</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Garantizar que el target `typecheck` existe y pasa en todos los proyectos del
workspace que compilan con `tsc`, no solo en `base-backend`.

## Entradas

- `tools/scripts/nx-typecheck-plugin.mjs`
- `nx.json` (targetDefaults de `typecheck`)

## Tareas

1. **Auditar cobertura actual**:
   - Ejecutar `pnpm nx show projects` y filtrar por proyectos sin target `typecheck`.
   - Identificar proyectos que deberían tenerlo pero no lo tienen.
2. **Agregar target faltante**:
   - Para proyectos con `tsconfig.lib.json` / `tsconfig.app.json` / `tsconfig.json`
     pero sin target `typecheck`, agregarlo manualmente o verificar que el plugin
     lo detecte.
3. **Corregir fallos**:
   - Ejecutar `pnpm typecheck:all` y arreglar errores de compilación.
4. **Verificar**:
   - `pnpm typecheck:all` pasa.
   - `pnpm typecheck:affected` pasa.

## Restricción

No modificar proyectos que usen `@nx/vite:typecheck` (Vite/React) — esos ya
tienen su propio target y no necesitan el wrapper de `tsc`.

## Criterios de aceptación

- [ ] Todos los proyectos Angular/backend tienen target `typecheck`.
- [ ] `pnpm typecheck:all` pasa.
- [ ] `pnpm typecheck:affected` pasa en una rama con cambios.

## Dependencias

- Ninguna.
