<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F51-C2 — Higiene artefactos + ignore `tsbuildinfo`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Evitar que artefactos de typecheck (`*.tsbuildinfo`) y similares entren en
commits / PRs. Alinear `.gitignore` y limpiar tracked si aplica.

## Resultado

- `.gitignore`: `*.tsbuildinfo` + `**/*.tsbuildinfo`.
- Untracked del índice: `apps/arquetipos/frontend/nextjs/next-{single,multi}/tsconfig.tsbuildinfo`
  (`git rm --cached`).
- `git ls-files '*.tsbuildinfo'` → vacío.

## Criterios de aceptación

- [x] Ningún `*.tsbuildinfo` tracked.
- [x] `.gitignore` cubre el patrón.
