<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F54-A3 — Deprecar primitivos framework-only en `@base/*-ui`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Tras F53 freeze + F54-A1 (menos usos), marcar como **`@deprecated`** los
primitivos Angular/React/Ionic de **base** que tienen equivalente Native*,
con JSDoc apuntando al wrapper, y opcional codemod / eslint.

**No** deprecar:

- Wrappers `Native*`.
- `@josanz/angular-ui` de marca (salvo re-exports que solo aliasen legacy base).
- `@base/react-native-ui` (runtime distinto; alinear por tokens B2).

## Tareas

1. Lista canónica “legacy → Native*” (Button, Alert, Input, …).
2. Añadir `@deprecated` + `eslint-plugin-deprecation` o ts-expect si ya hay
   patrón en el repo (`check:deprecated`).
3. Actualizar [deprecation-policy.md](../../../guides/deprecation-policy.md)
   con ventana (p. ej. remove no antes de F56).
4. Codemod opcional (`tools/scripts/codemod-native-wrappers.mjs`) features →
   Native*.
5. No borrar implementación en F54 (solo mark + docs).

## Criterios de aceptación

- [ ] ≥ 3 primitivos base marcados deprecated con alternativa clara.
- [ ] `pnpm check:deprecated` sigue pasando (o allowlist actualizada).
- [ ] Política de remove documentada (ronda mínima).
