<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F68-A3 — Ionic FeatureShell thin wrapper</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F68" src="https://img.shields.io/badge/round-F68-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Opcional post-[F67-A2](../plans-67-sixty-seven-round/1750000241000-f67-feature-shell-adoption-closeout.md):
pasar Ionic de **CSS-only** `arq-feature*` a un **thin wrapper** del
FeatureShell (misma API de slots / presentation) sin reintroducir
`MobileXPage` / `DesktopXPage`.

## Entregables

1. Decisión: wrapper Ionic **o** mantener CSS-only documentado.
2. Si wrapper:
   - componente thin que reutiliza markup/slots del shell web o CE;
   - piloto ≥1 dominio (clients o settings).
3. Actualizar [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md)
   (matriz stack × adopción).

## Criterios de aceptación

- [x] Decisión explícita en guía (wrapper **o** CSS-only intencional).
- [x] Si wrapper: ≥1 página Ionic sin markup chrome duplicado.
- [x] Sin páginas MobileX/DesktopX nuevas.

## Verificación

```bash
pnpm nx typecheck base-ionic-settings-features
# o dominio piloto Ionic equivalente
```

## Depende de

F67-A2 (CSS-only documentado como intencional).
