# F58-B3 — Remove framework `@deprecated` primitives (carry F57-D1)

## Estado

listo para ejecutar

## Objetivo

Eliminar o dejar de exportar primitivos framework-only marcados `@deprecated` en
F54 (tras migración native wrappers), con codemod/PR acotado.

## Criterios de aceptación

- [ ] Lista de símbolos eliminados + PR o defer F59 con dependencias restantes.
- [ ] `pnpm check:deprecated` / `check:ui-native-first:strict` verdes.
- [ ] Sin regresiones ownership UI.
