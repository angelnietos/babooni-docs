# Design system — UI del monorepo

Cuándo usarla: tokens, Storybook, ownership de componentes, handoff desde Figma.

**ADRs:** [0010 native-ui SoT](../adr/adr-0010-native-ui-lit-sot.md) ·
[0011 Storybook](../adr/adr-0011-storybook-native-ui-first.md) ·
[0006 capas FE](../adr/adr-0006-frontend-layering.md).

Estrategia operativa: [ui-strategy.md](./ui-strategy.md) (incluye **freeze F53+**).

## Capas de UI

| Paquete | Rol |
|---------|-----|
| `@base/native-ui` | **SoT** Lit Custom Elements (cross-framework) |
| `@base/angular-ui` / `@base/react-ui` | Wrappers `Native*` + legacy framework-only (congelado) |
| `@base/react-native-ui` | Primitivos RN (tokens/API alineados; no Lit) |
| `@josanz/angular-ui` | Marca Josanz (wrappers + re-exports) |
| `@arquetipos/*-ui` | Plantilla demo (wrappers premium / thin) |
| `@saas/shared-ui` | SaaS (re-export / extensión base) |

Decisión wrap vs re-export: [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md).

## Catálogo

Fuente máquina: [ui-component-catalog.yaml](./ui-component-catalog.yaml).

- Genérico → preferir `@base/native-ui` + wrappers; no nuevos solo-framework en base
- Branding Josanz → `@josanz/angular-ui`
- CRM SaaS → `@saas/shared-ui`
- **No mezclar** imports entre `layer:arquetipos` ↔ `layer:clientes` ↔ `layer:productos-saas`

Gate: `node tools/scripts/check-ui-ownership.mjs` (F53+: soft native-first).

## Storybook

| Proyecto | Serve (objetivo) | Build | Puerto (ref.) |
|----------|------------------|-------|----------------|
| `base-native-ui` | sí (F53-B2) | sí | ~4400 |
| `base-angular-ui` | sí (F53-B1) | sí (CI) | ~4402 |
| `base-react-ui` | sí (F53-B1) | sí (CI) | ~4403 |
| `josanz-angular-ui` | sí | sí | 4401 |

- Stories en el paquete **dueño** del componente.
- Plantillas Arquetipos: targets Nx o deprecación explícita (F53-D2).
- CI visual / Chromatic: F54-B1.

```bash
# Cuando existan targets F53:
pnpm nx storybook base-native-ui
pnpm nx storybook base-angular-ui
pnpm nx storybook base-react-ui
pnpm nx storybook josanz-angular-ui
```

## Figma / handoff

1. Diseñar con tokens/semántica alineados al catálogo (nombres de componente).
2. Marcar qué es **native / base wrapper** vs **variante de producto**.
3. Implementar: **CE en native-ui primero**; luego wrapper; re-export o wrap de marca.
4. Actualizar `ui-component-catalog.yaml` si el ownership cambia.
5. Code Connect ↔ native-ui: [F54-D1](../plans/rounds/plans-54-fifty-four-round/1750000108000-f54-figma-code-connect-native-ui.md).

## Tokens

Preferir variables CSS / theme del paquete UI. Evadir hex sueltos en features.
F54: paquete `@base/ui-tokens` compartido web↔RN.

## Verificación

```bash
node tools/scripts/check-ui-ownership.mjs
pnpm nx typecheck base-native-ui
pnpm nx typecheck josanz-angular-ui
```

## Enlaces

- [josanz-product-exceptions.md](./josanz-product-exceptions.md)
- [ci-gates.md](./ci-gates.md)
- Planes [F53](../plans/rounds/plans-53-fifty-three-round/) / [F54](../plans/rounds/plans-54-fifty-four-round/)
