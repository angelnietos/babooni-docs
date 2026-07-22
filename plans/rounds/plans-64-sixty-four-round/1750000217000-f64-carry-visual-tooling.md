<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-C2 — Carry: Chromatic / Code Connect / deprecated</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Ejecutar [F63-D2](../plans-63-sixty-three-round/1750000206000-f63-carry-visual-tooling.md) /
cadena F59-B1/B2/B3. Sin token Chromatic → alternativa o defer **con motivo**.

## Entregables

1. **Chromatic o alt:** token + publish `base-native-ui` **o** Percy / Playwright
   screenshots soft **o** defer F65 documentado en `design-system.md` + `ci-gates.md`.
2. **Code Connect:** `*.figma.ts` piloto button/input/alert/select **o** defer
   (sin acceso Figma).
3. **Deprecated:** inventario consumers Angular/React atoms `@deprecated` →
   migrar features arquetipos **o** lista residual con fechas.

## Criterios de aceptación

- [ ] Cada sub-ítem: hecho **o** defer F65 con owner/blocker.
- [ ] No dejar el plan en “listo” sin decisión al cerrar F64.

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
rg -n "@deprecated" libs/base/frontend --glob "*.ts" | head
```

## Depende de

F64-C1.
