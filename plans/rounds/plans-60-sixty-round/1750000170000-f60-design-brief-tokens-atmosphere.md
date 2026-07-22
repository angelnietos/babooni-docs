<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F60-A1 — Design brief + tokens atmósfera</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Fijar la **dirección visual** de la plataforma (átomos + features nativas) y
traducirla a tokens en `@base/ui-tokens` / CSS vars que ya consumen Lit CE.

Inspiración: Ubisoft / Nintendo / Rockstar **filtrada** → “cinematic HUD”
sobrio (atmósfera, jerarquía, un acento). No cosplay de marcas ni assets con
copyright.

## Entregables

1. Doc breve en `docs/frontend/` o `docs/design-system.md` (sección F60):
   - Paleta / contraste / tipografía / radios / elevación / motion (2–3
     intenciones: enter, focus, feedback).
   - Anti-patrones: púrpura genérico AI, cream+terracotta, glow, pills/stat
     strips de marketing, HUD saturado.
2. Tokens nuevos o ajustados en
   [`libs/base/frontend/crosscutting/ui-tokens`](../../../../libs/base/frontend/crosscutting/ui-tokens)
   (`tokens.ts` + `tokens.css` + espejo `rn.ts` si aplica).
3. Tabla “qué token usa cada átomo” (button/input/alert/card).
4. Capa CSS **7–1** [`@base/ui-styles`](../../../../libs/base/frontend/crosscutting/ui-styles)
   ([ui-styles-7-1.md](../../../frontend/ui-styles-7-1.md)).

## Criterios de aceptación

- [x] Brief mergeado y enlazado desde biblia / design-system.
- [x] Tokens consumibles por `@base/native-ui` sin romper temas arquetipos
      existentes (fallback a vars actuales).
- [x] RN: mismos nombres semánticos en `ui-tokens/rn` (ADR 0010).
- [x] `@base/ui-styles` 7–1 con `main.scss` + pages auth/clients.

## Verificación

```bash
pnpm nx typecheck base-ui-tokens   # o proyecto Nx equivalente
pnpm check:ui-native-first         # soft si ya existe
```

## Depende de

Nada (bloquea A2 / C1).

## Bloquea

F60-A2, F60-C1, F60-C2.
