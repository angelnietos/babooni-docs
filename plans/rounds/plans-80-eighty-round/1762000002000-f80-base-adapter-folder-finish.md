<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F80-B1 — Finish base adapter folder layout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F80" src="https://img.shields.io/badge/round-F80-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24 · Seed F79: next / ionic / rn **ya** en `atoms|chrome|composites|theme`

## Objetivo

Completar el mismo layout en el resto de UI base: `base-angular-ui`, `base-react-ui` (`wrappers/native` → opcionalmente `lib/atoms/{name}/Native*`), y cualquier flat residual.

## Acciones

- [x] Auditar `src/lib/**` de cada `base-*-ui`
- [x] Flats React → `chrome/{error-boundary,permissions,tenant-switcher}`; excepción `wrappers/native` documentada
- [x] Actualizar barrels `index.ts` sin romper exports públicos
- [x] `tsc --noEmit` / typecheck por proyecto (fallback cuando Nx daemon EPERM)

## Criterios

- [x] Ningún `.ts(x)` suelto en `src/lib/` (salvo allowlist documentada — vacía)
- [x] Storybook globs `../src/**/*.stories.*` siguen encontrando stories
