<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F80-A1 — UI lib layout contract + check</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F80" src="https://img.shields.io/badge/round-F80-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24

## Objetivo

Fijar el contrato `atoms/ | chrome/ | composites/ | theme/` + carpeta por componente, y un check Nx/CI que falle si hay `.ts(x)` sueltos en `src/lib/`.

## Acciones

- [x] Publicar `docs/frontend/ui-lib-folder-layout.md` (contrato + ejemplos Next/Ionic/RN ya migrados)
- [x] Enlazar desde design-system + matriz adapters
- [x] `tools/checks/check-ui-lib-layout.mjs` — soft (CI) → strict local
- [x] Allowlist vacía tras B* (sin packages mid-migration)

## Criterios

- [x] Check documentado en `docs/frontend/ci-gates.md`
- [x] Seed libs (`base-next-ui`, `base-ionic-ui`, `base-react-native-ui`) pasan strict
