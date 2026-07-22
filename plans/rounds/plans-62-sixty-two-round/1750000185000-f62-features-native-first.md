<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F62-C2 — Features native-first (camino feliz sin framework UI)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

En `libs/arquetipos/**/features` y apps plantilla, el camino feliz **no importa**
`PageHeader` / `TableToolbar` / `ArqSelect` / `<select>` / botones framework
cuando exista equivalente Native*. Framework UI queda en `@arquetipos/*-ui` solo
como re-export legado o para casos sin CE aún.

## Entregables

1. Inventario: imports framework UI en features arquetipos → tabla migrate/keep.
2. Migrar clients (ya casi), users, roles, audit, settings, native-ui page.
3. Regla / check soft: `check-ui-ownership` o script `rg` documentado en F62-E1.
4. Actualizar [ui-strategy.md](../../../frontend/ui-strategy.md) / parity:
   “arquetipos demo = native-first”.

## Criterios de aceptación

- [ ] Users/Roles Angular+React sin `<select>` ni ArqSelect en templates.
- [ ] Documentado qué queda framework y por qué (YAGNI / gap CE).

## Verificación

```bash
rg '<select' libs/arquetipos/frontend --glob '**/features/**'
rg 'ArqSelectComponent|PageHeaderComponent' libs/arquetipos/frontend --glob '**/features/**'
```

## Depende de

F62-A2.

## Bloquea

F62-B1 (polish sobre native), F62-C1.
