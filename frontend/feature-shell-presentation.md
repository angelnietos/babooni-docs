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

Hoy el código reutiliza clases `arq-clients*` en otros dominios — deuda semántica.
Migrar a `arq-feature*` (o equivalente) + componente/layout shell.

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
| `board` | Kanban (Native UI board) |
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

Borrador de contrato en apertura F66-A3; implementación en la tarjeta.
