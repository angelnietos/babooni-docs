<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F52-A1 — Carry npm publish + versionado (post F51-E1)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

F51-E1 canario ya cerrado. Gaps residuales cerrados aquí:

| Gap | Entrega |
|-----|---------|
| Publish real | `pnpm publish:publishable` + workflow [release-publishable.yml](../../../../.github/workflows/release-publishable.yml) (`workflow_dispatch`, dry-run default) |
| Guía | [npm-publish-and-versioning.md](../../../guides/npm-publish-and-versioning.md) — Verdaccio, GH Actions, peers |
| Dry-run | `publish:publishable` → 3 tarballs semver, sin upload |
| Scopes producto | Sin publicar `@josanz` / `@saas` |

Residual = 0 para oleada canario. Ampliar oleada / registry product → futura ronda.

## Criterios de aceptación

- [x] Checklist F51-E1 100% + residual canario = 0.
- [x] `.npmrc.example` presente.
- [x] Pack + publish dry-run documentados.
- [x] No publicar scopes producto sin decisión.
