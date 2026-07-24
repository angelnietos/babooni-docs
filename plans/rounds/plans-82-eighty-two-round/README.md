<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F82 — Josanz on native SoT + product wrappers</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-81-eighty-one-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F81-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Activa** · apertura 2026-07-24

> Carry de [F81-E1](../plans-81-eighty-one-round/1763000005000-f81-josanz-folder-carry.md). Eje: la app Angular Josanz **se ve igual que ahora**, pero los controles compartidos **extienden `@base/native-ui`** vía adapters `@base/angular-ui` + wrappers de marca en `@josanz/angular-ui`. Lo no reutilizable queda **explícito en Josanz**. El flatten `atoms|composites` es parte de esta ronda (después o en paralelo seguro a los wrappers).

## Contexto

F81 cerró native-first en **Arquetipos**. Josanz ERP sigue con catálogo Angular propio (CSS/tema) sin consumir Lit SoT:

1. `@josanz/angular-ui` no importa `@base/native-ui` — p. ej. `josanz-button` es implementación propia.
2. Taxonomía producto (`forms/`, `feedback/`, `layout/`, `catalog/`, …) aún no está en `atoms|composites` físicos.
3. Hay solape conceptual con átomos SoT (button, input, select, alert, modal, badge, …) y mucho **solo-producto** (catalog list, settings panels, delete host, Figma shells).

Contrato objetivo:

```text
@base/native-ui (Lit SoT)
  → @base/angular-ui Native*
    → @josanz/angular-ui wrappers (brand / defaults ERP)
      → apps/clientes/josanz + @josanz/*-features

Product-only (no forzar a base) → permanece en @josanz/angular-ui
```

Regla wrap vs re-export: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md).

## Objetivos clave

1. Inventario: WRAP / RE-EXPORT / PROMOTE-TO-BASE / JOSANZ-ONLY.
2. Extender base native (+ adapters Angular) solo con gaps **reutilizables**.
3. Wrappers Josanz sobre Native* con **paridad visual** de la app actual.
4. Carry físico `forms|…` → `atoms|composites|theme`; barrels `@josanz/angular-ui` estables.
5. Docs + gates; cerrar F82.

## Planes detallados

| ID | Plan | Foco |
|----|------|------|
| F82-A1 | [Inventario native ↔ Josanz](1764000001000-f82-josanz-native-inventory.md) | Matriz decisión por componente |
| F82-B1 | [Extender base native](1764000002000-f82-extend-base-native.md) | Gaps reutilizables → SoT + adapters |
| F82-C1 | [Wrappers Josanz sobre Native*](1764000003000-f82-josanz-native-wrappers.md) | Paridad visual app Angular |
| F82-D1 | [Folder carry atoms\|composites](1764000004000-f82-josanz-folder-carry.md) | Move físico + barrels |
| F82-E1 | [Docs, gates y cierre](1764000005000-f82-docs-gates-close.md) | Hub planes + layout doc |

## Checklist de cierre F82

- [ ] F82-A1 matriz publicada (sin mover aún)
- [ ] F82-B1 promotes a base justificados y typecheck/test SoT verdes
- [ ] F82-C1 controles canario + oleada; app Josanz paridad visual
- [ ] F82-D1 sin taxonomía legacy suelta; `check-ui-lib-layout` OK
- [ ] F82-E1 docs + hub; **F82 cerrada**
- [ ] Cambios commitidos por plan (o hitos claros)

## Predecesora / Siguiente

- Predecesora: [F81](../plans-81-eighty-one-round/) (cerrada)
- ADRs: [0010](../../../adr/adr-0010-native-ui-lit-sot.md) · [0011](../../../adr/adr-0011-storybook-native-ui-first.md)
- Layout: [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md)
- Guía wrap: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md)
- Matriz adapters: [native-ui-adapter-matrix.md](../../../frontend/native-ui-adapter-matrix.md)
- App: `apps/clientes/josanz/frontend/josanz`
- Lib: `libs/clientes/josanz/angular-ui` (`@josanz/angular-ui`)
