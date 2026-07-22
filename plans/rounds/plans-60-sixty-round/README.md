<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F60 — Native UI cross-framework + atmósfera + Storybook</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

cerrada (carry → [F62](../plans-62-sixty-two-round/))

> Abierta 2026-07-22 · cerrada 2026-07-22. Entregó tokens + 7–1 `@base/ui-styles`,
> login/clients compartidos Angular/React/Next/Ionic, e2e SPA verdes.
> Pendiente visual (temas, select, features, showcase `/native-ui`) y carries
> E1/E2 → **F62**.

## Contexto

Angular y React arquetipos ya están relativamente cerca; Next, Ionic y RN se
ven y comportan distinto pese a ADR 0010 (native-ui SoT). F60 unifica **look +
flujos canario** vía:

1. Tokens + atmósfera (`@base/ui-tokens`).
2. Átomos Lit pulidos (`@base/native-ui`) + wrappers thin.
3. **Features nativas** (composiciones: login, clients list) consumidas por
   stacks DOM; RN espejo por tokens/API (no Lit en el árbol nativo).
4. Storybook native-first (ADR 0011) como contrato visual.

## Dirección visual (“cinematic HUD” sobrio)

Inspiración Ubisoft / Nintendo / Rockstar **filtrada** (no cosplay, no neon):

| Tomar | Evitar |
|-------|--------|
| Jerarquía tipográfica clara | Púrpura genérico / glow AI |
| Atmósfera de profundidad suave | Cream + terracotta cliché |
| Un acento contenido | Stat strips / pills de marketing |
| Motion corto (enter, focus, feedback) | HUD saturado / cromados estridentes |

Detalle en **F60-A1**.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F60-A1 | [Design brief + tokens atmósfera](1750000170000-f60-design-brief-tokens-atmosphere.md) | completado (+ 7–1 `@base/ui-styles`) |
| F60-A2 | [Polish átomos `@base/native-ui`](1750000171000-f60-native-ui-atoms-polish.md) | parcial → F62-D3 |
| F60-B1 | [Storybook native-ui completo](1750000172000-f60-storybook-native-ui-complete.md) | parcial → F62-D3 |
| F60-B2 | [Storybook adapters / brand](1750000173000-f60-storybook-adapters-brand.md) | trasladado → F62-D3 |
| F60-C1 | [Feature nativa Auth login](1750000174000-f60-native-feature-auth-login.md) | completado (DOM stacks) |
| F60-C2 | [Feature nativa Clients list canario](1750000175000-f60-native-feature-clients-list.md) | completado (DOM stacks) |
| F60-D1 | [Adoptar en apps arquetipos](1750000176000-f60-adopt-arquetipos-apps.md) | parcial (login/clients; resto → F62) |
| F60-E1 | [Carry F59: validación isomórfica](1750000177000-f60-carry-isomorphic-validation.md) | trasladado → F62-D1 |
| F60-E2 | [Carry F59: Chromatic / Code Connect / deprecated](1750000178000-f60-carry-visual-tooling-deprecated.md) | trasladado → F62-D2 |
| F60-F1 | [Docs polish + hub F60](1750000179000-f60-documentation-polish.md) | parcial → F62-E1 |

## Entregado (resumen)

- `@base/ui-styles` 7–1 + `arq-auth*` / `arq-clients*`
- Teal tokens; login/clients Angular·React·Next·Ionic
- E2e angular-single + react-single chromium OK

## Carry explícito → F62

Carry / cierre de la nota F60 “RN espejo tokens/API, no Lit” → **F62-A4**.

Temas tenant, native select listbox, polish users/roles/audit/settings/shell,
showcase `/native-ui` real, native-first features, átomos Lit, Storybook
adapters, Chromatic/Code Connect/deprecated, docs → **F62** (ver README F62).


## Fuera de alcance (histórico)

- Reescribir todos los dominios Josanz/SaaS de golpe.
- Lit dentro de React Native nativo.
- Assets / skins con copyright de publishers.
