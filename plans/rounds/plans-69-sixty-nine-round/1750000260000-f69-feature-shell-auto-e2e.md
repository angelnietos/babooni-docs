<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F69-A1 — FeatureShell auto presentation harden + e2e</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F69" src="https://img.shields.io/badge/round-F69-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Garantizar que `presentation: 'auto'` muestra **solo cards** &lt; 1024px y
**solo table** ≥ 1024px — sin listado duplicado. SoT en `@base/ui-styles`
`pages/feature-list` + e2e Ionic multi.

Seed: fix F68 (CSS SoT + `ArqIonFeatureShell` encapsulated rules + Playwright).

## Entregables

1. Contrato CSS en SoT + spec `seven-one` (data-presentation auto).
2. E2e desktop + narrow en `ionic-multi-e2e` (count=1 por cliente demo).
3. Guía [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md)
   matrix breakpoint × presentation al día.
4. Smoke users/roles Ionic si ya usan shell.

## Criterios de aceptación

- [ ] Desktop e2e: `.arq-feature__mobile-list` hidden; table visible; 1× cada cliente.
- [ ] Narrow e2e: cards visible; table hidden.
- [ ] Sin regressión Angular/React (`presentation="table"` sigue ok).

## Verificación

```bash
pnpm nx test base-ui-styles
pnpm nx e2e ionic-multi-e2e
```
