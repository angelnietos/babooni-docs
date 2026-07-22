# Design system — UI del monorepo

Cuándo usarla: tokens, Storybook, ownership de componentes, handoff desde Figma.

**ADRs:** [0010 native-ui SoT](../adr/adr-0010-native-ui-lit-sot.md) ·
[0011 Storybook](../adr/adr-0011-storybook-native-ui-first.md) ·
[0006 capas FE](../adr/adr-0006-frontend-layering.md).

Estrategia operativa: [ui-strategy.md](./ui-strategy.md) (incluye **freeze F53+**).

Ronda activa: [F62](../plans/rounds/plans-62-sixty-two-round/) — temas tenant,
átomos native-ui, shell/nav, RN tokens, `base-select` listbox, features
native-first, showcase `/native-ui`, Storybook/Chromatic/docs. F60 cerró
login/clients + 7–1 `@base/ui-styles`.

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
| Clients list | `@base/ui-styles` → `pages/_clients-list.scss` | `arq-clients*` |
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

- Stories en el paquete **dueño** del componente — **ver `base-native-ui` primero**.
- Inferencia: `@nx/storybook/plugin` en `nx.json`; puertos/styles/output en
  `project.json` por lib.
- **CI visual / Chromatic (F60-E2):** sin `CHROMATIC_PROJECT_TOKEN` →
  `build-storybook` de `base-native-ui` (+ angular/react-ui) en CI es la señal
  de rotura; Chromatic / Code Connect defer F61 si falta acceso.

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
F54: paquete `@base/ui-tokens` compartido web↔RN. F60: tipografía + motion + atmósfera.

## Verificación

```bash
node tools/checks/check-ui-ownership.mjs
pnpm check:ui-native-first
pnpm nx typecheck base-native-ui
pnpm nx typecheck base-ui-tokens
pnpm nx typecheck josanz-angular-ui
```

## Enlaces

- [josanz-product-exceptions.md](./josanz-product-exceptions.md)
- [ci-gates.md](./ci-gates.md)
- Planes [F60](../plans/rounds/plans-60-sixty-round/) (activa) · [F53](../plans/rounds/plans-53-fifty-three-round/) / [F54](../plans/rounds/plans-54-fifty-four-round/)
