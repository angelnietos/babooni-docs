<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">UI: re-export vs wrapper</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: al exponer un componente de `@base/*-ui` en una capa de producto
(`@josanz/angular-ui`, `@arquetipos/*-ui`, `@saas/shared-ui`).

> Preferir **re-exportar** cuando no hay marca, comportamiento, configuración ni
> template distinto. Crear **wrappers** solo cuando aportan valor real.

## Tabla de decisión

| Situación | Recomendado |
|-----------|-------------|
| Misma implementación; solo exponer desde otro paquete | Re-export |
| Branding propio (template, estilos, iconos) | Wrapper |
| Configuración por defecto distinta | Wrapper |
| Lógica adicional | Wrapper |
| “Quizá luego lo personalizamos” (sin cambio ahora) | Re-export (YAGNI) |

## Anti-simetría

**No** crear wrappers solo porque otro componente ya tiene wrapper. El wrapper de
`ButtonComponent` no obliga a envolver `TopbarComponent`. Evalúa cada uno.

## Principios

- **YAGNI** — no abstraer antes de necesitarlo.
- **DRY** — no duplicar implementación.
- **Menos mantenimiento** — menos código y tests.
- **Semántica** — un wrapper es una *variante* de producto, no un alias.

## Cómo aplicar

| Acción | Criterio | Ejemplo |
|--------|----------|---------|
| `RE-EXPORT` | Sin diferencia de marca | `TopbarComponent`, `AlertComponent` vía `@josanz/angular-ui` |
| `WRAP` | Marca / config / lógica distinta | `ButtonComponent` con template Josanz |
| `DEFER` | Custom esperado pero ausente | Re-export ahora; wrap después |

Nunca un wrapper que solo re-expone la base sin valor añadido.

## Relación con native-ui (F53+)

Antes de wrap/re-export de marca:

1. ¿El átomo genérico vive en `@base/native-ui`? → wrap `Native*` en base, luego
   marca.
2. ¿Solo existe primitivo framework-only legacy en base? → **no** inventes otro;
   abre CE + wrapper ([ADR 0010](../adr/adr-0010-native-ui-lit-sot.md)).
3. Wrapper de marca sobre `Native*` / CE = patrón preferido para capar branding.

## Catálogo y ownership

- Máquina de verdad: [ui-component-catalog.yaml](../frontend/ui-component-catalog.yaml)
- Gate CI: `pnpm check:ui-ownership` / `node tools/checks/check-ui-ownership.mjs`
- Excepciones Josanz: [josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md)
- Design system: [design-system.md](../frontend/design-system.md)
- Estrategia + freeze: [ui-strategy.md](../frontend/ui-strategy.md)

## Verificación

```bash
node tools/checks/check-ui-ownership.mjs
pnpm nx typecheck josanz-angular-ui   # o el paquete UI tocado
```

## Enlaces

- ADR [0006](../adr/adr-0006-frontend-layering.md) · [0010](../adr/adr-0010-native-ui-lit-sot.md)
- Rule de agente: `.cursor/rules/ui-wrap-decider.mdc` (apunta a esta guía)
