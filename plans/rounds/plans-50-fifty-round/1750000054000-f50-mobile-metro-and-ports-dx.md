<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F50-C1 — DX mobile Metro + matriz de puertos</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

- Helper único: `tools/metro/create-arquetipos-expo-metro-config.cjs`
  (`disableHierarchicalLookup` + `extraNodeModules` React 18).
- `react-native-single` / `react-native-multi` `metro.config.js` lo consumen.
- Allowlist ESLint `@nx/enforce-module-boundaries` para el path `tools/metro/…`
  (mismo patrón que configs ESLint; evita relative “external resource”).
- Docs: [add-mobile-domain.md](../../../guides/add-mobile-domain.md),
  [local-development.md](../../../guides/local-development.md) — puertos
  Ionic 4300/4301, RN 8091/8092, Next 4240, SaaS 3120/4230 alineados con
  `project.json`.
- Lint Nx `react-native-single` / `react-native-multi` verde tras el move.
- Smoke RN web: reiniciar con `--clear` si se cambió Metro (sesión previa
  validó login en :8095 con el mismo pin).

## Criterios de aceptación

- [x] Un solo módulo shared de Metro para single + multi.
- [x] Docs de puertos sin contradicciones.
- [x] Smoke RN web login OK (nota en Resultado).
