<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F81-E1 — Josanz folder carry (opcional)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F81" src="https://img.shields.io/badge/round-F81-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Defer → [F82](../plans-82-eighty-two-round/)** · 2026-07-24

Prioridad baja frente a A1–D1. El presupuesto de F81 se gastó en contrato native-first,
split `wrappers/native` → `atoms/{name}`, paridad de features y retirada de dual CSS.

## Objetivo

Si el presupuesto lo permite: mapear `@josanz/angular-ui` `forms|feedback|layout|…` a `atoms|composites` físicos. Si no: defer explícito a F82 sin bloquear cierre F81.

## Acciones

- [x] Estimar churn de imports Storybook/apps — alto (taxonomía producto ERP; F80-B2 ya documentó defer)
- [x] Documentar “→ F82” en README F81 checklist (no olvidado)
- [x] Ronda F82 abierta — scope ampliado: native-extend + wrappers + folder carry ([F82 README](../plans-82-eighty-two-round/README.md))
- [ ] Ejecutar move físico Josanz — **[F82-D1](../plans-82-eighty-two-round/1764000004000-f82-josanz-folder-carry.md)**

## Criterios

- [x] Hecho o defer explícito (no “olvidado”)

## Notas para F82

- Ronda: [plans-82-eighty-two-round](../plans-82-eighty-two-round/) — primero inventario + wrappers sobre `@base/native-ui`; folder carry en D1.
- Layout conceptual ya en [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md) (`@josanz/angular-ui`).
- Mantener barrels públicos estables; mover solo paths internos + Storybook globs.
- No bloquear con rename masivo de imports de apps si se puede re-exportar desde carpetas nuevas.
