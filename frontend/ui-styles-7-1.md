<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">UI styles — arquitectura 7–1 (`@base/ui-styles`)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

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
├── pages/       # composiciones canario (auth-login, feature-list, crud-page, …)
├── themes/      # platform + tenants.css + arq-atmosphere-overrides.css
├── vendors/     # terceros
└── main.scss
```

Clases: **BEM** (`block__element--modifier`), p. ej. `arq-auth__hero`.

**Import CSS en Vite:** PostCSS `@import '@base/ui-styles/…'` **no** resuelve
exports del paquete. Preferir `import '@base/ui-styles/pages/….css'` desde TS/JS
de la feature, o ruta relativa desde SCSS/CSS de Angular/Ionic.

Cascade tenant vs atmósfera (no invertir):
[tenant-themes-checklist.md](./tenant-themes-checklist.md).

Polish visual F69–F71 cerrado (solo en git). Contrato vivo:
[design-system.md](./design-system.md) · [feature-shell-presentation.md](./feature-shell-presentation.md).

## Relación con otras capas UI

| Paquete | Rol |
|---------|-----|
| `@base/ui-tokens` | Tokens JS + `tokens.css` + RN |
| `@base/ui-styles` | **7–1** SCSS/CSS compartido (esta capa) |
| `@base/native-ui` | Lit CE SoT; compositions CSS → alias a `ui-styles` |

Apps Angular cargan `main.scss` en `project.json` `styles`. Features pueden
importar solo una página (`pages/auth-login.css`, `pages/feature-list.css`).
Alias `pages/clients-list.css` = thin re-export de `feature-list` (F68-A2;
preferir `feature-list` en código nuevo).

## Verificación

```bash
pnpm nx test base-ui-styles
pnpm nx typecheck base-ui-styles
```

Ver también [design-system.md](./design-system.md).
