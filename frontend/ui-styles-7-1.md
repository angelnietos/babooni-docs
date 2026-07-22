# UI styles — arquitectura 7–1 (`@base/ui-styles`)

Cuándo usarla: estilos compartidos entre stacks arquetipos DOM para **paridad
visual**. Josanz ya tenía 7–1 en `@josanz/angular-ui`; F60 añade la capa base
equivalente.

## Patrón 7–1

Siete carpetas + un `main.scss` que las importa:

```
src/styles/
├── abstracts/   # variables, mixins (herramientas Sass)
├── base/        # reset, tipografía
├── components/  # helpers de host / CE
├── layout/      # shell / canvas
├── pages/       # composiciones canario (auth, clients; F62: users/roles/…)
├── themes/      # CSS custom properties (platform + tenant cascade F62)
├── vendors/     # terceros
└── main.scss
```

Clases: **BEM** (`block__element--modifier`), p. ej. `arq-auth__hero`.

**Import CSS en Vite:** PostCSS `@import '@base/ui-styles/…'` **no** resuelve
exports del paquete. Preferir `import '@base/ui-styles/pages/….css'` desde TS/JS
de la feature, o ruta relativa desde SCSS/CSS de Angular/Ionic.

Ronda activa de polish visual: [F62](../plans/rounds/plans-62-sixty-two-round/).

## Relación con otras capas UI

| Paquete | Rol |
|---------|-----|
| `@base/ui-tokens` | Tokens JS + `tokens.css` + RN |
| `@base/ui-styles` | **7–1** SCSS/CSS compartido (esta capa) |
| `@base/native-ui` | Lit CE SoT; compositions CSS → alias a `ui-styles` |

Apps Angular cargan `main.scss` en `project.json` `styles`. Features pueden
importar solo una página (`pages/auth-login.css`).

## Verificación

```bash
pnpm nx test base-ui-styles
pnpm nx typecheck base-ui-styles
```

Ver también [design-system.md](./design-system.md).
