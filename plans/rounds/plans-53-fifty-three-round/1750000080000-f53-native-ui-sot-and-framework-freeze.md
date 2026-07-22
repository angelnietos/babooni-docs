<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F53-A1 — Política native-first + freeze UI framework en base</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

**ADR de contrato:** [0010](../../../adr/adr-0010-native-ui-lit-sot.md) · Storybook
[0011](../../../adr/adr-0011-storybook-native-ui-first.md). Biblia ya actualizada
(`ui-strategy`, `design-system`, hub, deprecation-policy).

## Objetivo

Hoy coexisten:

1. **SoT correcta:** `@base/native-ui` (Lit CE) — 14 elementos.
2. **Deuda:** primitivos “nativos de framework” en `@base/angular-ui`,
   `@base/react-ui`, `@base/ionic-ui`, `@base/react-native-ui` que **duplican**
   trabajo visual por stack (5+ superficies).

**Decisión de producto (esta ronda):**

> A partir de **ahora**, no se añaden ni se evolucionan (features nuevas,
> variantes, rediseños) primitivos framework-only en base **salvo** bugs
> críticos de seguridad/regresión. El camino canónico es Lit en
> `@base/native-ui` + **wrappers** por framework que capan API, tipado,
> change detection o branding. En el **futuro** (F54+) se podrán retirar o
> reescribir los framework-only existentes; **no** es objetivo de F53 borrar
> el catálogo Angular/React actual.

Se integrarán componentes de **otros proyectos** ya existentes fuera del
monorepo (ver A3) **hacia** native-ui / wrappers, no hacia nuevas islas
framework.

## Tareas

1. Actualizar [ui-strategy.md](../../../frontend/ui-strategy.md):
   - Sección **Freeze (F53):** tabla “permitido / no permitido”.
   - Flujo “añadir primitivo” → **obligatorio** native-ui primero.
   - RN: excepción explícita (tokens/API, no Lit en native).
2. Actualizar [design-system.md](../../../frontend/design-system.md) +
   [how-it-works.md](../../../frontend/how-it-works.md) (1 párrafo cada uno).
3. Nota en [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md):
   wrappers sobre CE son el patrón preferido en base.
4. Comentario corto en `AGENTS.md` / platform-bible skill si el sync lo permite
   (o solo docs + plan; sync agents en F1).
5. Marcar en catálogo YAML (o README native-ui) qué componentes Angular/React
   son **legacy parallel** vs **wrapper de CE**.

## Criterios de aceptación

- [ ] Biblia deja inequívoco: no mantener nuevos primitivos framework-only.
- [ ] Wrappers / re-exports de marca siguen siendo válidos.
- [ ] RN documentado como paralelismo de tokens, no Lit.
- [ ] Enlace desde hub «cómo hago UI» → ui-strategy § freeze.
