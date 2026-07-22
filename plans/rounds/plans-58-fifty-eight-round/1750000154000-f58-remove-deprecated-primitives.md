<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F58-B3 — Remove framework `@deprecated` primitives (carry F57-D1)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F59 (consumers base/SaaS/stories siguen)

## Objetivo

Eliminar o dejar de exportar primitivos framework-only marcados `@deprecated` en
F54 (tras migración native wrappers), con codemod/PR acotado.

## Criterios de aceptación

- [x] Lista de símbolos eliminados + PR o defer F59 con dependencias restantes.
- [x] `pnpm check:deprecated` / `check:ui-native-first:strict` verdes.
- [x] Sin regresiones ownership UI.
