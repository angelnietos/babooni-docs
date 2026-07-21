# UI: re-export vs wrapper

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

## Catálogo y ownership

- Máquina de verdad: [ui-component-catalog.yaml](../frontend/ui-component-catalog.yaml)
- Gate CI: `pnpm check:ui-ownership` / `node tools/scripts/check-ui-ownership.mjs`
- Excepciones Josanz: [josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md)
- Design system: [design-system.md](../frontend/design-system.md)

## Verificación

```bash
node tools/scripts/check-ui-ownership.mjs
pnpm nx typecheck josanz-angular-ui   # o el paquete UI tocado
```

## Enlaces

- ADR [0006](../adr/adr-0006-frontend-layering.md)
- Rule de agente: `.cursor/rules/ui-wrap-decider.mdc` (apunta a esta guía)
