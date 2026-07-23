<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F72-A1 — Carry: Chromatic / Code Connect / deprecated atoms</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F72" src="https://img.shields.io/badge/round-F72-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Resolución de los carries heredados de F68 → F69 → F70 → F71.

| Sub-ítem | Blocker |
|----------|---------|
| Chromatic CI soft | Sin `CHROMATIC_PROJECT_TOKEN` / paquete chromatic |
| Code Connect | Sin acceso Figma CI |
| Migración auth + chrome → Native | Superficie amplia; arquetipos happy path ya Native-first |

## Entregables

1. Cada sub-ítem completamente resuelto y habilitado.
2. Actualizar `design-system.md` y `ci-gates.md` con la integración de Chromatic/Code Connect finalizada.
3. Inventario de deprecated atoms completado y residual cerrado.

## Criterios de aceptación

- [ ] Chromatic CI y Code Connect funcionando y configurados.
- [ ] `deprecated-atoms-residual.md` marcado como completamente cerrado. No se permite defer a F73.
