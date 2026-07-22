<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F45-A4 — Automatizar compatibilidad Node ↔ Nx</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Implementar el check automático de compatibilidad entre Node.js y Nx definido
en el plan global `F00-node-nx-compatibility.md`.

## Entradas

- `docs/plans/global/F00-node-nx-compatibility.md`
- Matriz de versiones documentada en F00.

## Tareas

1. **Definir matriz en código**:
   - Crear `tools/scripts/lib/node-nx-versions.mjs` con la matriz:
     ```js
     export const NX_NODE_MATRIX = {
       '23.x': ['20.x', '22.x'],
       '24.x': ['22.x', '24.x'],
     };
     ```
2. **Implementar `check:node-nx`**:
   - Crear `tools/scripts/check-node-nx.mjs` que:
     - Lea `nx --version` y extraiga la major.minor.
     - Lea `node --version` y extraiga la major.minor.
     - Verifique que Node esté en el rango permitido para esa versión de Nx.
     - Falle con mensaje claro si no es compatible.
   - Registrar en `package.json`:
     ```json
     "check:node-nx": "node tools/scripts/check-node-nx.mjs"
     ```
3. **Integrar en CI**:
   - Agregar `pnpm check:node-nx` al workflow de CI.
4. **Documentar recuperación**:
   - Actualizar `docs/guides/local-development.md` con el procedimiento:
     - Si el language server de Nx falla: recargar VS Code.
     - Si persiste: `pnpm nx reset`.
     - Si aún persiste: verificar versión de Node con `pnpm check:node-nx`.

## Restricción

No cambiar la matriz sin antes actualizar el plan global `F00`.

## Criterios de aceptación

- [ ] `pnpm check:node-nx` existe y pasa en la versión actual.
- [ ] `pnpm check:node-nx` falla si se simula una versión incompatible.
- [ ] CI ejecuta `check:node-nx` y pasa.
- [ ] Procedimiento de recuperación documentado en `docs/guides/local-development.md`.

## Dependencias

- F00 (plan global).
