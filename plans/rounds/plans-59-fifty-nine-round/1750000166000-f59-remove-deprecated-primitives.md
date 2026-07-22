<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F59-B3 — Remove framework `@deprecated` primitives (carry F58-B3)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F60-E2 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Dejar de exportar (o eliminar) Button/Input/Alert framework-only marcados
`@deprecated` tras migrar consumers restantes en base / stories / SaaS wrappers.

## Criterios de aceptación

- [ ] Inventario consumers → migrados a `Native*` **o** defer F60 con lista.
- [ ] `pnpm check:deprecated` / `check:ui-native-first:strict` verdes.
- [ ] Sin regresiones ownership UI.

## Notas

F58 defer: login-form, audit panels, stories, wrappers SaaS aún dependen de
exports legacy. Priorizar unexport barrel antes que borrar ficheros.
