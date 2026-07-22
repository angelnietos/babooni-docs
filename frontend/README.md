<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Frontend</h1>

<p align="center">
  Capas · Angular / React / Next / mobile · UI nativa (Lit) · paridad
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./ui-strategy.md"><img alt="UI strategy" src="https://img.shields.io/badge/UI-Lit%20SoT-14b8a6?style=flat-square" /></a>
  <a href="./design-system.md"><img alt="Design system" src="https://img.shields.io/badge/design-system-0d5f59?style=flat-square" /></a>
</p>

## Lectura recomendada

| Orden | Doc | Contenido |
|-------|-----|-----------|
| 1 | [how-it-works.md](./how-it-works.md) | 4 capas, apps thin, data flow |
| 2 | [ui-strategy.md](./ui-strategy.md) | Lit native-ui SoT + freeze + wrappers ([ADR 0010](../adr/adr-0010-native-ui-lit-sot.md)) |
| 3 | [../architecture/framework-decision-guide.md](../architecture/framework-decision-guide.md) | Cuándo Angular / React / Next / RN |
| 4 | [../architecture/frontend-deep-dive.md](../architecture/frontend-deep-dive.md) | Deep dive capas |
| 5 | [design-system.md](./design-system.md) | Tokens, Storybook ([ADR 0011](../adr/adr-0011-storybook-native-ui-first.md)), catálogo |
| 5b | [ui-styles-7-1.md](./ui-styles-7-1.md) | Arquitectura CSS 7–1 (`@base/ui-styles`) para paridad visual |
| 5c | [tenant-themes-checklist.md](./tenant-themes-checklist.md) | Identidad tenant vs atmósfera (Angular/React/Ionic) |
| 6 | [arquetipos-thin-libs.md](./arquetipos-thin-libs.md) | Thin libs plantilla |
| 7 | [workspace-packages.md](./workspace-packages.md) | exports ↔ paths |

## Referencia

| Doc | Contenido |
|-----|-----------|
| [josanz-product-exceptions.md](./josanz-product-exceptions.md) | audit/users thin, angular-ui |
| [tenant-themes-checklist.md](./tenant-themes-checklist.md) | Cascade `data-arq-tenant` + atmósfera secundaria |
| [ui-component-catalog.yaml](./ui-component-catalog.yaml) | Ownership componentes |
| [ci-gates.md](./ci-gates.md) | Gates FE |
| [dual-stack-clients-parity.md](./dual-stack-clients-parity.md) | Paridad Angular↔React |
| [tsconfig-paths-audit.md](./tsconfig-paths-audit.md) | Paths |

## Guías

- [add-frontend-domain.md](../guides/add-frontend-domain.md)
- [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md)
- [add-next-domain.md](../guides/add-next-domain.md)
- [add-mobile-domain.md](../guides/add-mobile-domain.md)
- Catálogo apps plantilla: [../arquetipos/](../arquetipos/README.md)
