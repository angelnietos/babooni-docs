<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F53-C2 — Gate soft: no nuevos primitivos solo-framework</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

La política A1 se olvida sin fricción mecánica. Añadir un **gate soft**
(warn / allowlist) que detecte **archivos nuevos** de componentes UI en
`@base/angular-ui` / `@base/react-ui` / `@base/ionic-ui` que **no** sean
wrappers `Native*` ni re-exports.

No fail CI el día 1 (mismo patrón que coverage soft F52).

## Tareas

1. Script `tools/scripts/check-ui-native-first.mjs` (o extensión de
   `check-ui-ownership`):
   - Heurística: paths `lib/**/button.component.ts` nuevos vs `native-*.ts`.
   - Allowlist de legacy existente (snapshot al abrir F53).
2. Script root `pnpm check:ui-native-first` (warn) y `:strict` (defer F54).
3. Documentar en pr-checklist + ci-gates (soft step opcional en job frontend).
4. Mensaje de error: “añade CE en @base/native-ui + wrapper; ver F53-A1”.

## Criterios de aceptación

- [ ] Script reproducible; legacy allowlist no falla.
- [ ] Doc + enlace a A1.
- [ ] Strict diferido con criterio (como coverage).
