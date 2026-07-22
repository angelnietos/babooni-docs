<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Feature shell — presentación genérica (no “clients UI”)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-68-sixty-eight-round/"><img alt="F68" src="https://img.shields.io/badge/F68-14b8a6?style=flat-square" /></a>
</p>

Contrato objetivo (F66-A3 + F68): **un chrome de feature reutilizable** para list CRUD
(y pantallas similares). Clients / users / roles / audit son **consumidores**,
no dueños del layout.

CSS SoT: `arq-feature*`. Dual `arq-clients*` **retirado** (F68-A2); path alias
`clients-list.css` → thin re-export de `feature-list.css`.

## Piezas

```
FeatureShell
├── header     title · subtitle · meta · primaryAction
├── toolbar    search · filters
├── body       PresentationStrategy
│                cards | table | board | custom
└── status     empty | loading | error
```

`[featureCustom]` / `customBody` se muestra también con lista vacía (create panel).

### PresentationStrategy

| Mode | Uso típico | Breakpoint |
|------|------------|------------|
| `cards` | Mobile / densidad táctil | fuerza cards |
| `table` | Desktop denso | fuerza table |
| `board` | Kanban (`<base-board>` / `NativeBoard`) | fuerza board |
| `auto` | cards &lt; desktop, table ≥ | CSS |
| `custom` | Dominio aporta el body entero | — |

Default: `auto`. Override por dominio (`roles` / `users` suelen forzar `table`).

**`auto` CSS (SoT):** `@base/ui-styles` `pages/feature-list` —
`.arq-feature[data-presentation='auto']` oculta table &lt; 1024px y cards ≥ 1024px.
Ionic: mismos selectores + reglas encapsuladas en `ArqIonFeatureShell`.
E2e: `ionic-multi-e2e` (desktop + narrow). Harden continuo → [F69-A1](../plans/rounds/plans-69-sixty-nine-round/1750000260000-f69-feature-shell-auto-e2e.md).

### Relación con otras guías

| Guía | Responsabilidad |
|------|-----------------|
| [features-layout-soc.md](./features-layout-soc.md) | Carpetas + dónde vive la lógica |
| [entity-view-abstractions.md](./entity-view-abstractions.md) | Campos read/write de **una entidad** |
| [state-soc-facade.md](./state-soc-facade.md) | Puerto de datos |
| **Esta** | Marco visual / presentación de la **lista** |

## Anti-patrones

- Copiar `home.page.css` de clients a roles cambiando el título.
- `MobileXPage` + `DesktopXPage` separados.
- Shell llamado `ClientsLayout` usado por users.
- Meter HTTP o validación de dominio en el shell.
- Regenerar dual-CSS naive (`@charset` / `@keyframes` / `var()` duplicados).

## Estado (F68)

| Pieza | Ubicación / estado |
|-------|--------------------|
| CSS `arq-feature*` | `@base/ui-styles` → `pages/feature-list` — dual `arq-clients*` retirado (F68-A2) |
| Path alias `clients-list*` | Thin re-export; apps/Next migrados a `feature-list` |
| Angular shell | `@base/angular-ui` → `FeatureShellComponent` — cards/table/**board**/auto/custom |
| React shell | `@base/react-shared` → `FeatureShell` — idem |
| Native board | `@base/native-ui` → `<base-board>` / `<base-board-lane>`; wrappers `NativeBoard` / `ArqNativeBoard` |
| Ionic | `@base/ionic-ui` → `ArqIonFeatureShell` (F68-A3); piloto clients `home.page` |
