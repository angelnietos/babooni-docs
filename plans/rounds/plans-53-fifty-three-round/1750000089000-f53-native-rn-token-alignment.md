<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F53-E1 — Tokens / API conceptual native ↔ React Native</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

RN **no** ejecuta Lit. Para no mantener 5 designs divergentes, alinear
**tokens y nombres de API** entre `@base/native-ui` y `@base/react-native-ui`
(size, variant, tone) sin compartir el runtime CE.

## Tareas

1. Extraer o documentar tokens CSS / consts compartidas (paquete fino
   `@base/ui-tokens` **o** archivo en native-ui exportado y consumido por RN
   donde sea viable sin DOM).
2. Tabla de paridad: props Button/Input/Alert native vs RN.
3. Gaps → issues / lista en Resultado (no hace falta cerrar todos en F53).
4. Nota en add-mobile-domain + ui-strategy.

## Criterios de aceptación

- [ ] Matriz de paridad publicada.
- [ ] Al menos tokens o consts compartidos **o** plan explícito “solo doc” si
      el extract es caro (justificar).
- [ ] Sin introducir Lit en bundle RN.
