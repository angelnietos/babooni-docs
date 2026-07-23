<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F74-C1 — Generator & CI Hardening</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F74" src="https://img.shields.io/badge/round-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

`tools/scaffolds/gen-domain.mjs` genera la infraestructura completa de un dominio nuevo
(y sus variantes por framework) incluyendo contratos isomórficos, ViewModel y 4 subcarpetas
no vacías. CI bloquea cualquier PR que rompa la paridad.

## Entregables

1. **`gen-domain.mjs` actualizado:**
   - Flags: `--domain <name>`, `--framework {angular,react,next,ionic,rn,all}`, `--viewmodel`.
   - Scaffolding de:
     - `@base/shared/src/lib/validation/<domain>.ts` y `.schema.ts` (stubs).
     - `libs/base/frontend/<fw>/<domain>/api/src/index.ts` (reexport limpio).
     - `libs/base/frontend/<fw>/<domain>/data-access/src/index.ts` (facade mock).
     - `libs/base/frontend/<fw>/<domain>/features/src/{layout,pages,components,logic}/`.
     - `logic/<Domain>Controller.ts` (stub) si `--viewmodel`.
2. **Test del generator:** `tools/scaffolds/__tests__/gen-domain.test.mjs` que:
   - Ejecuta `gen-domain.mjs --domain sample --framework next`.
   - Verifica existencia de archivos y ausencia de DTOs locales en api.
3. **CI gates (nx.json / GitHub Actions):**
   - `node tools/checks/check-domain-conventions.mjs` — obligatorio.
   - `node tools/checks/check-frontend-conventions.mjs` — obligatorio.
   - `pnpm typecheck:affected` — bloquea discrepancias de DTOs.
4. **Documentación en `docs/plans/README.md`**:
   - Checklist para crear un dominio nuevo sin omitir pasos.

## Reglas

- El generator no crea DTOs locales fuera de `@base/shared`.
- Archivos generados incluyen comentarios con link a la F74 aplicable.
