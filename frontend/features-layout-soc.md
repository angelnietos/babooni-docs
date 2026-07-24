<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Features layout + SoC (SOLID en paneles)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <img alt="F66-A1" src="https://img.shields.io/badge/F66--A1-14b8a6?style=flat-square" />
</p>

Contrato para libs `*-features` (base + arquetipos, web y mobile): **estructura
física** + **dónde vive la lógica**. Complementa el puerto data-access
([state-soc-facade.md](./state-soc-facade.md)).

## Estructura obligatoria

```
features/src/
├── layout/        # chrome del dominio (título, outlet, wrappers de sección)
├── pages/         # targets de ruta — thin: componen layout + components
├── components/    # UI smart: bindings a servicio/hook/facade; sin reglas de dominio
├── services/      # Angular / Ionic — orquestación de panel (opcional)
├── hooks/         # React / Next / RN — orquestación de panel (opcional)
├── {domain}.routes.ts
└── index.ts
```

Gate: `node tools/checks/check-frontend-conventions.mjs` (F66-A1 amplía cobertura
a Ionic / Next / RN si falta).

## SOLID en el panel (resumen)

| Principio | Aplicación |
|-----------|------------|
| **S** | Page = ruta; component = presentación; service/hook = orquestación; facade = datos |
| **O** | Extender vista/config (F66-A2) en vez de editar templates monolíticos |
| **L** | Subclases/hooks de dominio sustituibles por el contrato del padre |
| **I** | Facades estrechos (`load`/`create`/…); no “god store” en features |
| **D** | Features dependen del **puerto** (`ClientsFacade` / `useClientsFacade`), no de NgRx/RTK |

## Qué puede vivir en page/component

- Estado UI local: query string, modal open, selected tab.
- Wiring de eventos → llamadas al servicio/hook/facade.
- Composición de `@base/*-ui` / Native.

## Qué no

- Reglas de validación de dominio (usar predicates `@base/shared` / ADR 0012).
- HTTP directo / `ApiClient` (data-access).
- `Store` / `useAppDispatch` / arrays `DEMO_*` mutables para list CRUD.
- Duplicar mapping form↔DTO en tres stacks — un hook/servicio por stack o
  shared helper.

## Relación con A2 / A3

| Capa | Guía |
|------|------|
| Orquestación panel | esta (A1) + facade |
| Campos entidad read/write | [entity-view-abstractions.md](./entity-view-abstractions.md) (A2) |
| Chrome lista cards/table/board | [feature-shell-presentation.md](./feature-shell-presentation.md) (A3) — **no** reutilizar semántica `arq-clients*` |

## Verificación

```bash
node tools/checks/check-frontend-conventions.mjs
pnpm nx typecheck <project>-features
```
