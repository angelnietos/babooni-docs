<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F82-D1 — Josanz folder carry (atoms \| composites)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F82" src="https://img.shields.io/badge/round-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado · 2026-07-24

Carry explícito desde [F81-E1](../plans-81-eighty-one-round/1763000005000-f81-josanz-folder-carry.md).

## Objetivo

Mapear el árbol físico de `@josanz/angular-ui` a `atoms|composites|theme` según [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md), **sin romper** el barrel público.

## Mapeo orientativo

| Origen (taxonomía) | Destino |
|--------------------|---------|
| `actions/`, controles `forms/`, `feedback/` primitivo, badges/avatars | `atoms/{name}/` |
| `layout/`, `catalog/`, `settings/`, `overlays/` shells, `detail/`, `delete/` | `composites/{name}/` |
| `theme/`, `styles/` | `theme/` (mantener) |
| `services/`, `utils/`, `validators/`, `a11y/` | shared bajo `lib/` (no mezclar en atoms) |
| `docs/` | docs/stories (sin forzar atoms) |

Ajustar con matriz A1 (wrappers Native* viven en `atoms/{name}/`).

## Acciones

- [x] `git mv` por oleadas; actualizar relativos internos
- [x] Reescribir paths en `src/index.ts` (exports públicos iguales)
- [x] Storybook globs ya `../src/**/*.stories.*` — verificar tras move
- [x] `node tools/checks/check-ui-lib-layout.mjs` (+ strict si aplica)
- [x] Typecheck `josanz-angular-ui` + smoke app

## Criterios

- [x] No quedan carpetas `forms|feedback|layout|…` como taxonomía raíz (o documentadas como alias deprecados temporales)
- [x] Cero deep imports rotos hacia `@josanz/angular-ui/...` (API sigue siendo barrel)
- [x] Apps/features sin cambios de import de paquete
