<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F73-A2 — Cross-Framework Native UI & Universal Custom Elements Bridge</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F73" src="https://img.shields.io/badge/round-F73-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

---

## Objetivo

Consolidar la estrategia de **componentes visuales nativos isomórficos (Native UI)** basados en Lit Custom Elements y Web Components, garantizando soporte transparente en Server-Side Rendering (Next.js SSR / App Router) y adaptadores nativos dedicados para React Native.

---

## Motivación

Actualmente:
- En Next.js, los Lit Custom Elements de `@base/native-ui` pueden generar fallos en el bundler de Webpack (`__webpack_require__.n is not a function`) si se ejecutan directamente en Server Components.
- En React Native, los Web Components no son soportados nativamente por el motor de React Native UI sin un puente de primitivas JSX nativas.

---

## Solución Arquitectónica

1. **Next.js Client Bridge (`@base/next-native-ui`):**
   - Implementar wrappers client-side con carga diferida/dinámica (`'use client'`) e isomórficos con SSR fallback estilizado mediante HTML/CSS semántico puro con tokens CSS `--arq-*`.
2. **React Native Primitives Engine (`@base/react-native-ui`):**
   - Estandarizar la paridad de API entre los Custom Elements Web (`<arq-button>`, `<arq-input>`, `<arq-card>`) y los componentes React Native (`<ArqButton>`, `<ArqInput>`, `<ArqCard>`), compartiendo la misma biblioteca de tokens de diseño (`@base/design-tokens`).

---

## Entregables

1. Adaptador `@base/next-native-ui` sin errores de Webpack en Server Components.
2. Paridad total de props y eventos entre Custom Elements Web y Primitivas React Native.
3. Suite de pruebas visuales y de renderizado en Next.js, React, Angular e Ionic.

---

## Criterios de aceptación

- [ ] Next.js renderiza correctamente componentes Native UI sin fallos de compilación.
- [ ] React Native expone componentes visuales equivalentes a los Custom Elements Web.
- [ ] Los tokens de diseño (`--arq-*`) están sincronizados en los 5 entornos visuales.
