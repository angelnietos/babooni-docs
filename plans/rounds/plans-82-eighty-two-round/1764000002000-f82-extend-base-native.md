<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F82-B1 — Extender base native (gaps reutilizables)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F82" src="https://img.shields.io/badge/round-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · 2026-07-24 · depende de [F82-A1](1764000001000-f82-josanz-native-inventory.md)

## Objetivo

Donde el inventario marque **PROMOTE-TO-BASE**, enriquecer `@base/native-ui` (y adapters `@base/angular-ui` `Native*`) para que Josanz pueda wrappear sin reinventar CSS dual.

## Acciones

- [ ] Solo gaps con evidencia de reuso (Josanz + futuro Arquetipos/SaaS) — no subir brand Josanz a base
- [ ] Implementar/ajustar Lit CE + stories/tests SoT (`base-native-ui`)
- [ ] Actualizar adapters Angular `Native*` (y stories base si aplica)
- [ ] `pnpm nx test base-native-ui` + `pnpm nx typecheck` proyectos tocados
- [ ] Documentar API nueva en design-system / native-ui README si cambia contrato público

## Criterios

- [ ] Cada promote trazable a fila A1
- [ ] SoT tests + Storybook build verdes para átomos tocados
- [ ] Cero tokens `--josanz-*` en `@base/native-ui`
