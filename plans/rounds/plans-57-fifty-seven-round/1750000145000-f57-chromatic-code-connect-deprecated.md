# F57-D1 — Carry F56: Chromatic / Code Connect / remove deprecated

## Estado

listo para ejecutar

## Objetivo

Cerrar o re-diferir con motivo los tres carries de F55/F56:

1. **Chromatic** — visual regression Storybook (token `CHROMATIC_PROJECT_TOKEN`).
2. **Figma Code Connect** — mapear primitivos native-ui / wrappers.
3. **Remove `@deprecated`** — primitivos framework-only marcados en F54.

## Criterios de aceptación

- [ ] Cada ítem: **hecho** en F57 **o** defer F58 con bloqueo explícito (sin token / sin acceso Figma / riesgo de breaking).
- [ ] Si Chromatic: job soft o strict documentado; no romper PR sin token.
- [ ] Si remove deprecated: codemod o PR acotado + `check:deprecated` / native-first.
- [ ] Sin regresiones `check:ui-ownership:strict`.

## Notas

Sin secretos en git. Prefer soft CI hasta que el token esté en GitHub Actions secrets.
