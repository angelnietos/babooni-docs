<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F68-B1 — Carry: Chromatic / Code Connect / deprecated atoms</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F68" src="https://img.shields.io/badge/round-F68-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado (defer F69 — Chromatic / Code Connect / migración auth+chrome)

## Objetivo

Retomar el defer de F67-B1 documentado en
[deprecated-atoms-residual.md](../../../frontend/deprecated-atoms-residual.md):

| Sub-ítem | Blocker (F67) |
|----------|---------------|
| Chromatic CI soft | Sin `CHROMATIC_PROJECT_TOKEN` / package |
| Code Connect | Sin acceso Figma CI |
| Migración auth + chrome → Native | Superficie amplia; happy path arquetipos ya Native-first |

## Entregables

1. Chromatic: token + package + job soft en CI **o** defer F69 (owner:
   design-system).
2. Code Connect: mapear ≥1 oleada CE ↔ wrappers **o** defer F69.
3. Migrar residuales prioritarios: auth login form + presenters chrome
   (`ErrorState`, `ConfirmDialog`, `SearchBar`, `SessionExpiry`) →
   `Native*` / `<base-*>`.
4. Actualizar inventario residual; [design-system.md](../../../frontend/design-system.md)
   + [ci-gates.md](../../../frontend/ci-gates.md).

## Criterios de aceptación

- [x] Cada sub-ítem: hecho **o** defer F69 con owner/blocker en residual note.
- [x] No marcar “listo” sin decisión por fila.
- [x] Inventario refleja consumers reales post-migración.

## Verificación

`design-system.md` · `ci-gates.md` · `deprecated-atoms-residual.md` ·
Storybook a11y smoke si aplica.

## Depende de

Secretos / Figma; no bloquea A*.
