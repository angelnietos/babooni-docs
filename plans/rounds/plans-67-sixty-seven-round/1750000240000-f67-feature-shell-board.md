<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F67-A1 — FeatureShell board strategy</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F67" src="https://img.shields.io/badge/round-F67-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado (defer F68)

## Objetivo

Cerrar el defer de F66-A3: exponer `presentation: 'board'` en FeatureShell
cuando el board nativo (Native UI / Lit) esté listo; si no, documentar defer
F68 con owner + blocker (no silenciar).

Contrato: [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md).

## Decisión (2026-07-22)

**Defer F68-A1.** No existe base-board CE / wrapper Angular·React usable.
Owner: design-system / FE platform. Blocker: no Native board CE.

## Entregables

1. Inventario: ¿existe board CE / wrapper Angular·React usable en listas?
2. Si sí:
   - Strategy `board` en Angular `FeatureShellComponent` + React `FeatureShell`.
   - Piloto en 1 dominio arquetipos (p. ej. roles o un board demo) o story.
   - Actualizar guía (matrix presentation × breakpoint).
3. Si no: defer F68 en la guía + residual note con owner (design-system / FE).

## Criterios de aceptación

- [x] Decisión explícita: implementado **o** defer F68 (owner + blocker).
- [x] Si implementado: `presentation: 'board'` tipado; smoke visual ≥1 stack.
- [x] Guía `feature-shell-presentation.md` refleja el estado real.

## Verificación

```bash
pnpm nx typecheck base-angular-ui
pnpm nx typecheck base-react-shared
# visual: story / piloto board si aplica
```

## Depende de

F66-A3 (shell cards/table). Native board CE si existe.

## Carry

[F68-A1](../plans-68-sixty-eight-round/1750000250000-f68-feature-shell-board.md)
