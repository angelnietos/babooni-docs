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

Gate: `node tools/checks/check-ui-ownership.mjs` (F53+: soft native-first).

## Storybook

| Proyecto | Serve | Build | Puerto |
|----------|-------|-------|--------|
| `base-native-ui` | `nx storybook base-native-ui` | sí (CI) | 6007 |
| `base-angular-ui` | `nx storybook base-angular-ui` | sí (CI) | 4402 |
| `base-react-ui` | `nx storybook base-react-ui` | sí (CI) | 4403 |
| `josanz-angular-ui` | `nx storybook josanz-angular-ui` | sí | 4401 |
| `arquetipos-angular-ui` | `nx storybook arquetipos-angular-ui` | sí | 4404 |
| `arquetipos-react-ui` | `nx storybook arquetipos-react-ui` | sí | 4405 |

- Stories en el paquete **dueño** del componente.
- Inferencia: `@nx/storybook/plugin` en `nx.json`; puertos/styles/output en
  `project.json` por lib.
- **CI visual / Chromatic (F55-B1):** **defer F56** — sin `CHROMATIC_PROJECT_TOKEN`.
  Estrategia elegida: Playwright screenshots sobre Storybook static cuando haya
  presupuesto; mientras, `build-storybook` de `base-native-ui` (+ angular/react-ui)
  en CI es la señal de rotura de stories. Loki no adoptado (mantenimiento baselines).

```bash
pnpm nx storybook base-native-ui
pnpm nx build-storybook base-native-ui
pnpm nx storybook base-angular-ui
pnpm nx storybook base-react-ui
pnpm nx storybook josanz-angular-ui
pnpm nx storybook arquetipos-angular-ui
pnpm nx storybook arquetipos-react-ui
```

Tokens compartidos: `@base/ui-tokens` (+ `./css`, `./rn`). A11y:
[native-ui-a11y-matrix.md](./native-ui-a11y-matrix.md). Figma:
[figma-native-ui-map.md](./figma-native-ui-map.md).

## Figma / handoff

1. Diseñar con tokens/semántica alineados al catálogo (nombres de componente).
2. Marcar qué es **native / base wrapper** vs **variante de producto**.
3. Implementar: **CE en native-ui primero**; luego wrapper; re-export o wrap de marca.
4. Actualizar `ui-component-catalog.yaml` si el ownership cambia.
5. Code Connect ↔ native-ui: [figma-native-ui-map.md](./figma-native-ui-map.md).

## Tokens

Preferir variables CSS / theme del paquete UI. Evadir hex sueltos en features.
F54: paquete `@base/ui-tokens` compartido web↔RN.

## Verificación

```bash
node tools/checks/check-ui-ownership.mjs
pnpm nx typecheck base-native-ui
pnpm nx typecheck josanz-angular-ui
```

## Enlaces

- [josanz-product-exceptions.md](./josanz-product-exceptions.md)
- [ci-gates.md](./ci-gates.md)
- Planes [F53](../plans/rounds/plans-53-fifty-three-round/) / [F54](../plans/rounds/plans-54-fifty-four-round/)
