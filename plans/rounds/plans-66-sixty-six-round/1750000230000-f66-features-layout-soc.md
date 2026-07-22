<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F66-A1 — Features SoC + layout / pages / components</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F66" src="https://img.shields.io/badge/round-F66-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Aplicar **SOLID** y separación de responsabilidades en el front de **arquetipos**
(y `@base/*-features` que las plantillas reexportan):

1. Extraer **lógica de negocio / orquestación** de components y pages hacia
   **servicios** (Angular / Ionic) o **hooks** (React / Next / RN).
2. Garantizar estructura canónica `layout/` · `pages/` · `components/` en
   **todas** las libs `*-features` (web + mobile + Next) usadas por apps
   arquetipos.

Extiende F65-D1 (facade data-access): el puerto ya existe; ahora el **panel** no
mezcla UI con reglas (validación local, confirm/toast orchestration, mapping de
form ↔ DTO, filtros).

## Entregables

### A1.1 — Guía

- [features-layout-soc.md](../../../frontend/features-layout-soc.md) (contrato).
- Actualizar [add-frontend-domain.md](../../../guides/add-frontend-domain.md) y
  [add-mobile-domain.md](../../../guides/add-mobile-domain.md): carpeta
  `services/` o `hooks/` bajo features; pages solo componen.

### A1.2 — Estructura features

Inventario + fix gaps:

```
features/src/
├── layout/       # chrome del dominio (header, outlet)
├── pages/        # targets de ruta — thin
├── components/   # presentación / wiring UI → servicio|hook
├── services/     # Angular/Ionic: orquestación (opcional si cabe en facade)
├── hooks/        # React/Next/RN: orquestación (opcional si cabe en facade)
├── *.routes.ts
└── index.ts
```

Ampliar `tools/checks/check-frontend-conventions.mjs` a Ionic / Next / RN
features si hoy solo cubre Angular/React web.

### A1.3 — Piloto extracción

Dominios **clients** + **users** (mínimo):

| Stack | Extraer a |
|-------|-----------|
| Angular | `ClientsPanelService` / métodos del facade ya usados; page sin `save`/`remove` inline |
| React | `useClientsPanel` (confirm + toast + form map) |
| Ionic | servicio o métodos en facade; `HomePage` solo UI |
| Next / RN | hook panel sobre `useClientsFacade` |

## Criterios de aceptación

- [ ] Guía publicada y enlazada desde hub FE + guides.
- [ ] `check-frontend-conventions` (o extensión) verde para features arquetipos
      listadas en el plan.
- [ ] Piloto clients (+ users) en ≥2 stacks: page/component sin reglas de
      mutación/validación duplicadas — viven en servicio/hook/facade.
- [ ] Tests unitarios del servicio/hook (≥1 stack) con mock del facade.

## Verificación

```bash
node tools/checks/check-frontend-conventions.mjs
pnpm nx typecheck base-angular-clients-features
pnpm nx typecheck base-react-clients-features
pnpm nx typecheck base-ionic-clients-features
pnpm nx test base-react-clients-features
pnpm nx test base-ionic-clients-features
```

## Depende de

F65-D1 (facades). Guía [state-soc-facade.md](../../../frontend/state-soc-facade.md).

## Fuera de alcance

- Reescribir Josanz features en esta tarjeta (pueden adoptar el patrón después).
- Sustituir facades por otra herramienta de estado.
