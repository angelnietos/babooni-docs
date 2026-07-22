<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F66-A3 — Generic feature shell (presentation, not “clients UI”)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F66" src="https://img.shields.io/badge/round-F66-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Sustituir el anti-patrón actual (`arq-clients*` CSS / chrome copiado en roles,
users, audit…) por un **shell de feature genérico** (main layout + presentación)
**agnóstico de dominio**, capaz de:

- Cambiar **modo de render** (lista cards / tabla desktop / board / empty /
  loading) sin reescribir el page.
- Adaptar **presentación** mobile vs desktop (breakpoints / density) de forma
  estándar.
- Recibir **slots** (header, toolbar/search, primary action, row/card, footer)
  — el dominio solo aporta config + templates de celda.

Esto **no** es “una vista genérica de clientes”. Clients es un **consumidor**
del shell, igual que users/roles.

A2 sigue siendo el contrato de **campos** read/write (`EntityViewConfig`); A3
es el **marco de página** (layout + presentation strategy).

## Enfoque recomendado (composición > herencia de página)

Preferir **composition + config** sobre una mega-clase `ClientsPageBase`:

```
FeatureShell / CrudFeatureLayout
  ├── header (title, subtitle, meta, primary CTA)
  ├── toolbar (search slot, filters slot)
  ├── body
  │     PresentationStrategy: 'cards' | 'table' | 'board' | 'custom'
  │     (auto: cards < breakpoint, table ≥ desktop — overrideable)
  └── empty / error / loading slots
```

| Pieza | Vive en | Rol |
|-------|---------|-----|
| `FeatureShell` (Angular/Ionic component / React layout) | `@base/*-ui` o `ui-styles` + thin wrappers | Chrome + slots |
| `arq-feature*` CSS (rename from `arq-clients*`) | `@base/ui-styles` | Tokens / breakpoints |
| Domain page | `*-features` | Pasa facade data + cell renderers |
| Entity form/detail | A2 | Campos read/write; montado en modal/panel slot |

**Mobile/desktop:** un solo layout; la strategy cambia el body. No forks
`MobileClientsPage` / `DesktopClientsPage`.

## Entregables

### A3.1 — Contrato + guía

- [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md)
- Diagrama slots + matrix presentation × breakpoint.
- Decisión documentada: rename gradual `arq-clients*` → `arq-feature*` (alias
  CSS temporal OK).

### A3.2 — Implementación base

1. Shell Angular + React (web) con modes `cards` | `table`.
2. Ionic reutiliza las mismas clases `arq-feature*` (ion-searchbar ya alineado).
3. Piloto: **clients** + **roles** (o users) montados sobre el shell — cero
   markup de chrome duplicado.

### A3.3 — Board (opcional en ronda)

Si el board nativo (Native UI) ya existe, exponer `presentation: 'board'` como
strategy; si no cabe, defer F67 con motivo.

## Criterios de aceptación

- [ ] Guía publicada; hubs FE enlazan A3.
- [ ] ≥2 dominios usan el shell genérico (no clases/`arq-clients` semánticas
      nuevas).
- [ ] Mobile cards + desktop table vía strategy/breakpoint sin pages duplicadas.
- [ ] A2 entity form se monta en slot modal/panel del shell en al menos 1 dominio.
- [ ] Alias deprecado `arq-clients*` documentado o eliminado al cerrar.

## Verificación

```bash
pnpm nx typecheck base-angular-clients-features
pnpm nx typecheck base-ionic-roles-features
pnpm nx typecheck base-react-clients-features
# visual smoke: ionic-multi /clients + /roles — mismo chrome, distinto dominio
```

## Depende de

F66-A1 (estructura features). F66-A2 (campos). F65 confirm/toast.

## Fuera de alcance

- Design system nuevo / Storybook Chromatic (B1).
- Generador visual drag-and-drop de layouts.
- Forzar board en todos los dominios.
