<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F81-C1 — Features native parity (all runtimes)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F81" src="https://img.shields.io/badge/round-F81-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24

## Objetivo

Que auth / clients / users / roles / audit (y demos native-ui) en **Angular · React · Next · Ionic · RN** usen el **mismo set conceptual de átomos SoT**, vía el adapter del runtime — misma UX salvo chrome de plataforma.

## Oleadas por dominio

| Wave | Dominios | Done when |
|------|----------|-----------|
| W1 | auth, clients | Solo `ArqNative*` / `Arq*` adapter / `ArqIon*` SoT; sin `ButtonComponent` / `ArqButton` CSS |
| W2 | users, roles, audit | Idem |
| W3 | settings, native-ui demo, shells layout | Topbar/shell OK como chrome; átomos SoT nativos |

## Acciones

- [x] Matriz dominio × runtime × imports actuales (`rg` ArqNative vs ArqButton vs Button)
- [x] Sustituir imports en `libs/arquetipos/frontend/**/features/` (W1–W3: badge → `ArqNativeBadge*`)
- [x] Verificar thin UI next/ionic/rn re-exportan base suficiente (features thin → `@base/*-features` SoT)
- [x] Smoke: gate `check:ui-native-first:strict` verde (allowlist vacía)
- [x] `pnpm check:ui-ownership` + native-first soft→strict en allowlist shrinking

## Criterios

- [x] W1 verde en 5 runtimes aplicables
- [x] Ningún feature nuevo puede importar legacy (gate F81-A1)
- [x] Nota en [arquetipos-thin-libs.md](../../../frontend/arquetipos-thin-libs.md): “features → adapters SoT only”
