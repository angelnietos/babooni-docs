# Design system — UI del monorepo

Cuándo usarla: tokens, Storybook, ownership de componentes, handoff desde Figma.

## Capas de UI

| Paquete | Rol |
|---------|-----|
| `@base/angular-ui` / `@base/react-shared` / `@base/react-native-ui` | Primitivos genéricos |
| `@josanz/angular-ui` | Marca Josanz (wrappers + re-exports) |
| `@arquetipos/*-ui` | Plantilla demo (wrappers premium / thin) |
| `@saas/shared-ui` | SaaS (re-export / extensión base) |

Decisión wrap vs re-export: [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md).

## Catálogo

Fuente máquina: [ui-component-catalog.yaml](./ui-component-catalog.yaml).

- Genérico → `@base/*-ui`
- Branding Josanz → `@josanz/angular-ui`
- CRM SaaS → `@saas/shared-ui`
- **No mezclar** imports entre `layer:arquetipos` ↔ `layer:clientes` ↔ `layer:productos-saas`

Gate: `node tools/scripts/check-ui-ownership.mjs`.

## Storybook

Donde exista (`*.stories.ts(x)`), documenta variantes del componente de **ese**
paquete (base o producto). No dupliques stories de base dentro de Josanz salvo
que el wrapper cambie la API visual.

## Figma / handoff

1. Diseñar con tokens/semántica alineados al catálogo (nombres de componente).
2. Marcar qué es **base** vs **variante de producto**.
3. Implementar: re-export si idéntico; wrapper si branding.
4. Actualizar `ui-component-catalog.yaml` si el ownership cambia.
5. Code Connect / plugins Figma: usarlos si el equipo los tiene configurados;
   no son requisito de CI hoy.

## Tokens

Preferir variables CSS / theme del paquete UI (`arq` theme en RN, tokens Angular/React
existentes). Evadir hex sueltos en features; van en UI o theme.

## Verificación

```bash
node tools/scripts/check-ui-ownership.mjs
pnpm nx typecheck josanz-angular-ui
```

## Enlaces

- [josanz-product-exceptions.md](./josanz-product-exceptions.md)
- ADR [0006](../adr/adr-0006-frontend-layering.md)
- [ci-gates.md](./ci-gates.md)
