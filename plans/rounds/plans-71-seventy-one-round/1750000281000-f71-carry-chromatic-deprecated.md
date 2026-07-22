<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F71-A1 — Carry: Chromatic / Code Connect / deprecated atoms</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F71" src="https://img.shields.io/badge/round-F71-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Resolución de los carries heredados de F68 → F69 → F70.

| Sub-ítem | Blocker |
|----------|---------|
| Chromatic CI soft | Sin `CHROMATIC_PROJECT_TOKEN` / paquete chromatic |
| Code Connect | Sin acceso Figma CI |
| Migración auth + chrome → Native | Superficie amplia; arquetipos happy path ya Native-first |

## Entregables

1. Cada sub-ítem: hecho **o** defer F72 (owner + blocker en residual note).
2. Si Chromatic/Code Connect se habilitan: actualizar `design-system.md` / `ci-gates.md`.
3. Si deprecated migration avanza: inventario actualizado.

## Criterios de aceptación

- [ ] Decisión explícita por fila (no “listo” silencioso).
- [ ] residual `deprecated-atoms-residual.md` apunta a F72 o marca done.
