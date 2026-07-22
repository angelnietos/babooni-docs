<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F44-A3 — Tooling / CI para detectar deprecated</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Agregar detección automática de uso de APIs deprecadas en build y tests, y un job en CI que bloquee merge si se usan símbolos prohibidos.

## Entradas

- Resultado de F44-A1 (inventario de deprecations).
- Política definida en F44-A2.

## Tareas

1. Crear `tools/scripts/check-deprecated-usage.mjs`:
   - Escanea imports de símbolos deprecados.
   - Usa AST con `ts-morph` o regex como fallback.
   - Exit code 1 si encuentra usos activos de deprecated sin mark `allowed`.
   - Soportar `// allowed-deprecated: <razón>` como excepción explícita.
2. Agregar script a `package.json` raíz:
   - `"check:deprecated": "node tools/scripts/check-deprecated-usage.mjs"`
3. Agregar job en CI (GitHub Actions / pipeline existente):
   - Corre en PRs.
   - Bloquea merge si hay usos prohibidos.
4. Documentar el script y el job en `docs/guides/deprecation-policy.md`.

## Criterios de aceptación

- [ ] `pnpm check:deprecated` existe y pasa en main.
- [ ] CI ejecuta `check:deprecated` y falla si hay usos prohibidos.
- [ ] Documentado en guía de deprecación.

## Dependencias

- F44-A1
