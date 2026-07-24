<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F80 — UI lib folder layout, product wrappers & dead-atom purge</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-79-seventy-nine-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Cerrada** · 2026-07-24

> Ejecutada 2026-07-24. Eje: **ordenar libs UI por tipo + componente**, **alinear wrappers de producto con `apps/`**, y **inventario de átomos no usados** en Arquetipos / clientes / SaaS.

## Contexto

F79 migró Next/Ionic/RN a Lit SoT / tokens. Quedó deuda estructural:

1. **Archivos sueltos** en `src/lib/*.tsx` (Next / Ionic / RN) vs patrón canónico de `native-ui` / `angular-ui` (`lib/{component}/`).
2. Wrappers de marca (`@arquetipos/*-ui`, `@josanz/angular-ui`, `@saas/shared-ui`) no reflejan la taxonomía de apps (`arquetipos` · `clientes` · `productos-saas`).
3. Catálogo UI hinchado: componentes legacy / forks no referenciados por features de apps.

Referencia apps ↔ libs:

| Apps | Libs UI esperadas |
|------|-------------------|
| `apps/arquetipos/**` | `@base/*-ui` + thin `@arquetipos/*-ui` |
| `apps/clientes/**` | `@base/*-ui` + brand `@josanz/*-ui` (u otro cliente) |
| `apps/productos-saas/**` | `@base/*-ui` + `@saas/shared-ui` (+ Josanz puntual) |

## Contrato de layout (obligatorio)

```text
{ui-lib}/src/
├── index.ts                 # barrel público estable
└── lib/
    ├── atoms/               # SoT design atoms (Lit wrap / RN twin)
    │   └── {atom}/
    │       ├── {Component}.{ts,tsx}
    │       ├── {Component}.stories.*   # opcional
    │       └── index.ts                # opcional
    ├── chrome/              # platform only (ion-tabs, shells, searchbar…)
    │   └── {chrome}/
    ├── composites/          # page-header, table toolbar, confirm…
    │   └── {composite}/
    └── theme/               # tokens helpers + CSS vars
```

**Prohibido:** nuevos `.ts(x)` sueltos directamente bajo `src/lib/`.

Seed F79→F80: `base-next-ui`, `base-ionic-ui`, `base-react-native-ui` ya migrados a este árbol. Resto de libs UI → oleadas F80-B*.

## Objetivos clave

1. Publicar contrato + check (`check-ui-lib-layout`).
2. Completar layout en **todas** las UI libs (`base` Angular/React wrappers, brand arquetipos/josanz/saas).
3. Organizar brand wrappers por carpeta de producto alineada a `apps/`.
4. Inventario de uso → **borrar** componentes no usados (con deprecation 0 rondas si cero refs).
5. Absorber carries F79-C1/D1/E1 (Storybook parity, Arquetipos sameness, CI gates).

## Planes detallados

| ID | Plan | Foco | Estado |
|----|------|------|--------|
| F80-A1 | [UI lib layout contract + check](1762000001000-f80-ui-lib-layout-contract.md) | Docs + gate soft→strict | Hecho |
| F80-B1 | [Finish base adapter folders](1762000002000-f80-base-adapter-folder-finish.md) | Angular/React `wrappers/native` + residual flats | Hecho |
| F80-B2 | [Brand UI folders ↔ apps](1762000003000-f80-brand-ui-folders-apps.md) | arquetipos / josanz / saas structure | Hecho |
| F80-C1 | [Dead atom purge](1762000004000-f80-dead-atom-purge.md) | Delete unused across stacks | Hecho |
| F80-D1 | [F79 carries: SB + Arquetipos + CI](1762000005000-f80-f79-carries-sb-ci.md) | C1/D1/E1 deferred | Hecho |

## Checklist de cierre F80

- [x] F80-A1 contrato + check en CI (soft; strict local)
- [x] F80-B1 base adapters sin flats bajo `src/lib/` (`wrappers/native` documentado)
- [x] F80-B2 brand UI folders alineadas a apps (Josanz taxonomy → F81 flatten)
- [x] F80-C1 inventario + política keep; sin deletes agresivos de adapters matriz
- [x] F80-D1 Storybook stories adapters + CI `build-storybook` 6 pkgs
- [x] Cambios commitidos
- [x] **F80 cerrada**

## Predecesora / Siguiente

- Predecesora: [F79](../plans-79-seventy-nine-round/)
- ADRs: [0010](../../../adr/adr-0010-native-ui-lit-sot.md) · [0011](../../../adr/adr-0011-storybook-native-ui-first.md)
- Matriz: [native-ui-adapter-matrix.md](../../../frontend/native-ui-adapter-matrix.md)
- Siguiente: [F81](../plans-81-eighty-one-round/) — Arquetipos native-ui everywhere + wrappers → atoms/{name}
