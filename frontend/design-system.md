<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Design system — UI del monorepo</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: tokens, Storybook, ownership de componentes, handoff desde Figma.

**ADRs:** [0010 native-ui SoT](../adr/adr-0010-native-ui-lit-sot.md) ·
[0011 Storybook](../adr/adr-0011-storybook-native-ui-first.md) ·
[0006 capas FE](../adr/adr-0006-frontend-layering.md).

Estrategia operativa: [ui-strategy.md](./ui-strategy.md) (incluye **freeze F53+**).

UI layout: [ui-lib-folder-layout.md](./ui-lib-folder-layout.md) · Josanz ↔ native
(**F82** cerrada — wrappers `Native*` + `atoms|composites`; ver esta guía y
[ui-strategy.md](./ui-strategy.md)).

### Overlays → portal (F63 / F64-A1)

Product overlays that must escape shell clipping (`backdrop-filter`, `transform`,
`overflow` on stage/topbar) **portal to `document.body`**. Canonical example:
`<base-select>` listbox (`data-base-select-portal`). Do not re-root panels inside
blurred chrome.

**CSS compartido:** arquitectura [7–1](./ui-styles-7-1.md) en `@base/ui-styles`
(`main.scss` + abstracts/base/components/layout/pages/themes/vendors).

## F60/F62 — dirección visual (“cinematic HUD” sobrio)

Inspiración Ubisoft / Nintendo / Rockstar **filtrada** (sin cosplay ni assets con
copyright).

| Tomar | Evitar |
|-------|--------|
| Jerarquía tipográfica clara (IBM Plex Sans) | Púrpura genérico / glow AI |
| Atmósfera de profundidad suave (hero teal ink) | Cream + terracotta cliché |
| Un acento contenido (`--color-brand` teal `#0f766e`) | Stat strips / pills de marketing |
| Motion corto: enter / focus / feedback | HUD saturado / cromados |

### Tokens

Paquete: `@base/ui-tokens` (`tokenValues`, `tokens.css`, `./rn`).

| Grupo | Ejemplos |
|-------|----------|
| Color | `--color-fg`, `--color-brand`, `--color-cta`, `--color-hero-from/to` |
| Superficie | `--color-surface`, `--color-surface-muted`, `--shadow-*` |
| Tipo | `--font-sans`, `--text-sm`…`--text-hero` |
| Motion | `--duration-enter`, `--duration-focus`, `--duration-feedback`, `--ease-out` |

Alias `--arq-*` en `tokens.css` puentean apps arquetipos existentes.

### Qué token usa cada átomo canario

| Átomo | Tokens principales |
|-------|--------------------|
| `base-button` | `--color-cta*`, `--radius-md`, `--focus-ring`, `--duration-*`, `--shadow-*` |
| `base-input` | `--color-border/brand`, `--focus-ring`, `--color-fg/muted` |
| `base-alert` | `--status-*-bg/fg/border`, `--radius-lg` |
| `base-card` | `--color-surface/border`, `--shadow-sm`, `--radius-lg` |
| `base-badge` / `base-chip` | `--status-*` / `--color-brand*` |
| `base-spinner` / `base-avatar` | `--color-brand`, `--avatar-bg` |

### Composiciones compartidas (paridad apps)

| Composición | CSS SoT (7–1) | Clases BEM |
|-------------|---------------|------------|
| Auth login | `@base/ui-styles` → `pages/_auth-login.scss` | `arq-auth*` |
| Feature list | `@base/ui-styles` → `pages/_feature-list.scss` | `arq-feature*` (alias `clients-list*` **retirado F69-B3**) |
| Users / Roles / Audit / Settings | F62-B1 | pendiente F62 |
| Shell / tenant switcher | F62-B2 | pendiente F62 |

Entry: `@use '@base/ui-styles'` / `main.scss`. Alias legacy:
`@base/native-ui/compositions/*.css` → mismas pages.

Las features de cada stack **importan** esos CSS y rellenan con wrappers `Native*` /
`ArqNative*`. No inventar HTML/CSS de login/list en paralelo.

**Native-first (F62):** camino feliz arquetipos = `@base/native-ui`. Sin
`<select>` HTML (usar `base-select` listbox). Temas tenant deben cambiar brand /
CTA / nav de forma obvia.

## Capas de UI

| Paquete | Rol |
|---------|-----|
| `@base/native-ui` | **SoT** Lit Custom Elements (cross-framework) |
| `@base/ui-styles` | **7–1** SCSS compartido (themes + pages BEM) |
| `@base/angular-ui` / `@base/react-ui` | Wrappers `Native*` + legacy framework-only (congelado) |
| `@base/next-ui` | Next client islands → `Native*` / Lit CE (F79) |
| `@base/ionic-ui` | Átomos SoT → Lit; chrome `ion-*` documentado |
| `@base/react-native-ui` | Primitivos RN (tokens/API alineados; no Lit) |
| `@josanz/angular-ui` | Marca Josanz (wrappers + re-exports); taxonomía producto bajo `src/lib/*` — ver [ui-lib-folder-layout.md](./ui-lib-folder-layout.md) |
| `@arquetipos/*-ui` | Plantilla demo — `atoms|composites|theme` (adapters SoT en `atoms/{name}/`, F81) |
| `@saas/shared-ui` | SaaS (`atoms|composites` + re-export base) |

Decisión wrap vs re-export: [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md).

## Catálogo

Fuente máquina: [ui-component-catalog.yaml](./ui-component-catalog.yaml).

- Genérico → preferir `@base/native-ui` + wrappers; no nuevos solo-framework en base
- Branding Josanz → `@josanz/angular-ui`
- CRM SaaS → `@saas/shared-ui`
- **No mezclar** imports entre `layer:arquetipos` ↔ `layer:clientes` ↔ `layer:productos-saas`

Gate: `node tools/checks/check-ui-ownership.mjs` · native-first features:
`pnpm check:ui-native-first` (F81-A1 — scopes angular/react/next/ionic/rn;
allowlist ratchet hasta F81-C1/D1).

## Storybook

Matriz **SoT Lit + adapters en paridad** (ADR [0011](../adr/adr-0011-storybook-native-ui-first.md)):

| Proyecto | Renderer | Extensión | Puerto | Rol |
|----------|----------|-----------|--------|-----|
| `base-native-ui` | `@storybook/web-components-vite` | `.ts` + Lit `html` | 6007 | **Catálogo SoT** (custom elements) |
| `base-angular-ui` | `@storybook/angular` | `.ts` | 4402 | Wrappers Angular sobre Lit |
| `base-react-ui` | `@storybook/react-vite` | `.tsx` | 4403 | Wrappers React sobre Lit |
| `base-next-ui` | `@storybook/react-vite` | `.tsx` | 4406 | Adapters Next (client / CE islands) |
| `base-ionic-ui` | `@storybook/angular` | `.ts` | 4407 | Adapters Ionic sobre Lit / Ion |
| `base-react-native-ui` | `@storybook/react-vite` (+ RN-web) | `.tsx` | 4408 | Gemelos token/API (sin Lit en RN) |
| `josanz-angular-ui` | `@storybook/angular` | `.ts` | 4401 | Brand wrappers |
| `arquetipos-angular-ui` / `arquetipos-react-ui` | Angular / React | `.ts` / `.tsx` | 4404 / 4405 | Brand plantilla |

**Paridad:** el set de átomos documentado en cada adapter debe alinearse con
`base-native-ui` (Button, Input, Alert, …). Arquetipos en todos los runtimes
debe verse igual porque consume la misma SoT Lit (RN: tokens + misma API).

El catálogo SoT (`base-native-ui`) lleva **Docs autodocs** (`@storybook/addon-docs`),
**Controls** por propiedad pública, **Actions** en eventos `base-*`, página
`Introduction`, y stories Playground + variantes por átomo. Ver
[`libs/base/frontend/crosscutting/native-ui/README.md`](../../libs/base/frontend/crosscutting/native-ui/README.md).

- Inferencia: `@nx/storybook/plugin` en `nx.json`; puertos/styles/output en
  `project.json` por lib.
- **CI visual / Chromatic:** sin token → `build-storybook` native + adapters
  es la señal de rotura (ver [ci-gates.md](./ci-gates.md)).

```bash
pnpm storybook:native-ui
pnpm storybook:base-angular
pnpm storybook:base-react
pnpm storybook:base-next
pnpm storybook:base-ionic
pnpm storybook:base-rn
```

Tokens compartidos: `@base/ui-tokens` (+ `./css`, `./rn`). Adapter matrix:
[native-ui-adapter-matrix.md](./native-ui-adapter-matrix.md). UI folder layout:
[ui-lib-folder-layout.md](./ui-lib-folder-layout.md). Next SSR islands:
[next-native-ui.md](./next-native-ui.md). A11y:
[native-ui-a11y-matrix.md](./native-ui-a11y-matrix.md). Figma:
[figma-native-ui-map.md](./figma-native-ui-map.md). Validación isomórfica:
[ADR 0012](../adr/adr-0012-isomorphic-validation.md). Estado / SoC facade:
[state-soc-facade.md](./state-soc-facade.md).
Features SoC + layout: [features-layout-soc.md](./features-layout-soc.md).
Entity field views: [entity-view-abstractions.md](./entity-view-abstractions.md).
FeatureShell presentación: [feature-shell-presentation.md](./feature-shell-presentation.md).

## Figma / handoff

1. Diseñar con tokens/semántica alineados al catálogo (nombres de componente).
2. Marcar qué es **native / base wrapper** vs **variante de producto**.
3. Implementar: **CE en native-ui primero**; luego wrapper; re-export o wrap de marca.
4. Actualizar `ui-component-catalog.yaml` si el ownership cambia.
5. Code Connect ↔ native-ui: [figma-native-ui-map.md](./figma-native-ui-map.md).

## Tokens

Preferir variables CSS / theme del paquete UI. Evadir hex sueltos en features.
F54: paquete `@base/ui-tokens` compartido web↔RN. F60: tipografía + motion + atmósfera.
Arquetipos multi: **identidad tenant** gana sobre atmósfera tipográfica —
[tenant-themes-checklist.md](./tenant-themes-checklist.md).

## Verificación

```bash
node tools/checks/check-ui-ownership.mjs
pnpm check:ui-native-first          # soft (warn + allowlist)
pnpm check:ui-native-first:strict   # CI hygiene — ratchet only shrinks
pnpm nx typecheck base-native-ui
pnpm nx typecheck base-ui-tokens
pnpm nx typecheck josanz-angular-ui
```

**F81 native-first contract:** Arquetipos features → SoT adapters only
([arquetipos-thin-libs.md](./arquetipos-thin-libs.md)). Adapters in
`atoms/{name}/` ([ui-lib-folder-layout.md](./ui-lib-folder-layout.md)).
Allowlist: `tools/checks/ui-native-first-allowlist.json`.
## Enlaces

- [josanz-product-exceptions.md](./josanz-product-exceptions.md)
- [ci-gates.md](./ci-gates.md)
- UI cerrada (**F79–F82**): esta guía · [ui-strategy.md](./ui-strategy.md)
  (rondas en git). Activas: [Ideauto migration](../ideauto/migration/) ·
  [DB providers](../runbooks/database-providers.md) · [ADR 0014](../adr/adr-0014-database-provider-portability.md).
  Índice: [plans/README.md](../plans/README.md).
