<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F60-B2 — Storybook adapters / brand</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Completar huecos de Storybook en wrappers y brand **solo donde cambian la API
visual** (ADR 0011: no re-story átomos base sin valor).

## Alcance

| Paquete | Qué documentar |
|---------|----------------|
| `@base/angular-ui` `Native*` | Binding props/events; ejemplos OnPush |
| `@base/react-ui` `Native*` | `base-click` → `onClick` |
| `@arquetipos/arquetipos-*-ui` | Solo brand/host class distinto |
| Ionic / Next UI | Si exponen wrapper propio |

## Entregables

1. Inventario: stories existentes vs wrappers con delta visual.
2. Stories mínimas para deltas; borrar/orphan configs sin target Nx.
3. Enlace desde design-system: “ver native-ui primero”.

## Criterios de aceptación

- [ ] Serve `storybook` en base Angular/React UI documentado.
- [ ] Sin duplicar catálogo completo de átomos Lit en brand packages.

## Verificación

```bash
pnpm nx run base-angular-ui:build-storybook
pnpm nx run base-react-ui:build-storybook
```

## Depende de

F60-B1 (recomendado).
