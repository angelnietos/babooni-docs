<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Feature shell — presentación genérica (no “clients UI”)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-66-sixty-six-round/1750000237000-f66-generic-feature-shell.md"><img alt="F66-A3" src="https://img.shields.io/badge/F66--A3-14b8a6?style=flat-square" /></a>
</p>

Contrato objetivo (F66-A3): **un chrome de feature reutilizable** para list CRUD
(y pantallas similares). Clients / users / roles / audit son **consumidores**,
no dueños del layout.

CSS SoT: `arq-feature*`. Alias `arq-clients*` deprecado — retirada cuando
consumers=0 ([F68-A2](../plans/rounds/plans-68-sixty-eight-round/1750000251000-f68-retire-arq-clients-alias.md)).

## Piezas

```
FeatureShell
├── header     title · subtitle · meta · primaryAction
├── toolbar    search · filters
├── body       PresentationStrategy
│                cards | table | board | custom
└── status     empty | loading | error
```

### PresentationStrategy

| Mode | Uso típico |
|------|------------|
| `cards` | Mobile / densidad táctil |
| `table` | Desktop ≥ breakpoint |
| `board` | Kanban (Native UI board) — **defer F68-A1** (no base-board CE) |
| `custom` | Dominio aporta el body entero |

Default: `auto` → `cards` bajo breakpoint, `table` en desktop. Override por
dominio (`roles` puede forzar `table` always).

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

## Estado

Implementado (F66-A3 + F67-A2):

| Pieza | Ubicación / estado |
|-------|--------------------|
| CSS `arq-feature*` (+ alias `arq-clients*`) | `@base/ui-styles` → `pages/feature-list`; alias retire → F68-A2 |
| Angular shell | `@base/angular-ui` → `FeatureShellComponent` — **clients / roles / users** montados |
| React shell | `@base/react-shared` → `FeatureShell` — **clients / roles / users** montados |
| Ionic | CSS-only `arq-feature*` (intencional F67); thin wrapper opcional → [F68-A3](../plans/rounds/plans-68-sixty-eight-round/1750000252000-f68-ionic-feature-shell-wrapper.md) |

`presentation: 'board'` → [F68-A1](../plans/rounds/plans-68-sixty-eight-round/1750000250000-f68-feature-shell-board.md)
(Native board CE + strategy). Owner: design-system / FE platform. Blocker: no
base-board CE.
