<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F80-B2 — Brand UI folders ↔ apps abstraction</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F80" src="https://img.shields.io/badge/round-F80-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24 (Josanz full `atoms|composites` flatten → F81)

## Objetivo

Que los wrappers de producto reflejen la misma abstracción que `apps/`:

```text
apps/arquetipos/**     → libs/arquetipos/frontend/**/ui  (@arquetipos/*-ui)
apps/clientes/{slug}/** → libs/clientes/{slug}/**/ui     (@josanz/angular-ui, …)
apps/productos-saas/** → libs/productos-saas/**/ui       (@saas/shared-ui, …)
```

Dentro de cada brand UI:

```text
src/lib/
  atoms/        # brand wrap sobre base Native*/Arq* (solo si aporta marca)
  composites/   # shells de producto
  re-exports/   # thin re-export de base sin carpeta por simetría (YAGNI)
```

Regla: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md) — no wrappers vacíos.

## Acciones

- [x] Inventario `@arquetipos/*-ui`, `@josanz/angular-ui`, `@saas/shared-ui`
- [x] Reorganizar arquetipos + saas a `atoms|composites` (+ `theme` / `wrappers`)
- [x] Josanz: styles → `theme/`; taxonomía producto documentada (flatten F81)
- [x] Documentar mapa app → UI package en [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md)
- [x] Verificar `check-ui-ownership`

## Criterios

- [x] Estructura legible espejo de apps
- [x] Features de cada app importan solo su capa de brand + base
