<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F55-D1 — Figma Code Connect piloto (button / input / alert)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

defer F56

## Objetivo

F54-D1 publicó la [matriz Figma↔CE](../../../frontend/figma-native-ui-map.md)
sin herramienta. F55 crea mappings Code Connect para el piloto si hay acceso.

## Tareas

1. Verificar token/MCP Figma.
2. Si hay acceso: Code Connect para `base-button`, `base-input`, `base-alert`.
3. Si no: defer F56 + mantener matriz; no bloquear ronda.
4. Regla docs: Figma → native-ui primero (sin wrappers framework generados).

## Criterios de aceptación

- [x] Piloto Code Connect **o** defer explícito (sin token).
- [x] Sin regresiones ownership UI.

## Resultado

Defer Code Connect: no hay token Figma. Matriz actualizada con select / icon /
pagination; piloto Code Connect → F56.
