<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F52-B1 — Storybook Josanz: runtime tag + lib-layout warn</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

- Heurística: `…/clientes/*/storybook` → `runtime:angular` en `check-lib-layout.mjs`.
- Tags `josanz-storybook`: `layer:clientes`, `runtime:angular`, `type:tooling`
  (quitado `runtime:isomorphic` / `scope:client-angular`).
- Doc: [josanz-product-exceptions.md](../../../frontend/josanz-product-exceptions.md) § F52-B1.
- `check-lib-layout --strict`: 0 warn storybook.

## Criterios de aceptación

- [x] 0 warns storybook.
- [x] `--strict` no falla por storybook.
- [x] Excepción/heurística documentada.
