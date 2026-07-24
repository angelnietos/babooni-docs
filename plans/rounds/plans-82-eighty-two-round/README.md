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

**Cerrada** · 2026-07-24

> Carry de [F81-E1](../plans-81-eighty-one-round/1763000005000-f81-josanz-folder-carry.md). Eje: la app Angular Josanz **se ve igual que ahora**, pero los controles compartidos **extienden `@base/native-ui`** vía adapters `@base/angular-ui` + wrappers de marca en `@josanz/angular-ui`. Lo no reutilizable queda **explícito en Josanz**. Flatten `atoms|composites` ejecutado en D1.

## Contexto

F81 cerró native-first en **Arquetipos**. F82 llevó Josanz ERP al mismo contrato SoT:

1. Inventario WRAP / RE-EXPORT / PROMOTE / JOSANZ-ONLY.
2. Promotes canario en Lit (`outline`/`xl`, alert dismissible/neutral, checkbox description, badge neutral).
3. Wrappers canario sobre `Native*` + `registerNativeUi` en app/Storybook.
4. Carry físico a `atoms|composites|theme`.

Contrato:

```text
@base/native-ui (Lit SoT)
  → @base/angular-ui Native*
    → @josanz/angular-ui wrappers (brand / defaults ERP)
      → apps/clientes/josanz + @josanz/*-features

Product-only (no forzar a base) → permanece en @josanz/angular-ui
```

Regla wrap vs re-export: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md).
Matriz: [josanz-native-inventory.md](../../../frontend/josanz-native-inventory.md).

## Planes detallados

| ID | Plan | Estado |
|----|------|--------|
| F82-A1 | [Inventario native ↔ Josanz](1764000001000-f82-josanz-native-inventory.md) | hecho |
| F82-B1 | [Extender base native](1764000002000-f82-extend-base-native.md) | hecho |
| F82-C1 | [Wrappers Josanz sobre Native*](1764000003000-f82-josanz-native-wrappers.md) | hecho |
| F82-D1 | [Folder carry atoms\|composites](1764000004000-f82-josanz-folder-carry.md) | hecho |
| F82-E1 | [Docs, gates y cierre](1764000005000-f82-docs-gates-close.md) | hecho |

## Checklist de cierre F82

- [x] F82-A1 matriz publicada (sin mover aún)
- [x] F82-B1 promotes a base justificados y typecheck/test SoT verdes
- [x] F82-C1 controles canario + oleada parcial; app Josanz on Native*
- [x] F82-D1 sin taxonomía legacy suelta; `check-ui-lib-layout` OK
- [x] F82-E1 docs + hub; **F82 cerrada**
- [ ] Cambios commitidos por plan (o hitos claros) — a petición del usuario

## Deferred / follow-up

- Select + Modal WRAP (sheet CE) — oleada posterior
- Toast / Toggle / EmptyState / Skeleton → Native*
- `base-textarea` y demás promotes deferidos en inventario
- Merge `styles/` → `theme/` completo

## Predecesora / Siguiente

- Predecesora: [F81](../plans-81-eighty-one-round/) (cerrada)
- Paralelas activas: [F83](../plans-83-eighty-three-round/) · [F84](../plans-84-eighty-four-round/)
- ADRs: [0010](../../../adr/adr-0010-native-ui-lit-sot.md) · [0011](../../../adr/adr-0011-storybook-native-ui-first.md)
- Layout: [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md)
- App: `apps/clientes/josanz/frontend/josanz`
- Lib: `libs/clientes/josanz/angular-ui` (`@josanz/angular-ui`)
