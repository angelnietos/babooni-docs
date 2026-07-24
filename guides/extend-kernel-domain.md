<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Extender un dominio kernel (`@base/*`)</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


Cuando el cambio beneficia a **todos** los productos (Josanz, Ideauto, plantillas, futuros clientes), va en el kernel — no en `@josanz/*` / `@ideauto/*`.

---

## ¿Kernel o producto?

| Pregunta | Si sí → kernel |
|----------|----------------|
| ¿Lo usaría un segundo cliente sin reglas Josanz/Ideauto? | `@base/backend` / `@base/angular-*` |
| ¿Es branding o regla comercial de un cliente? | `@josanz/*` o `@ideauto/*` |
| ¿Es solo demo de plantilla? | `@arquetipos/*` thin re-export |

---

## Backend kernel

### Dominio nuevo en hex

```bash
node tools/scaffolds/new-domain.mjs widgets --dry
node tools/scaffolds/new-domain.mjs widgets
node tools/checks/check-domain-conventions.mjs
```

Exporta `WidgetsModule` desde `libs/base/backend/src/index.ts`.

### Extender dominio existente

1. Caso de uso en `hex/application/{domain}/`.
2. Puerto en `hex/domain/{domain}/` si necesitas nuevo contrato de persistencia.
3. Adaptador en `hex/infrastructure/{domain}/`.
4. Tests en `*.spec.ts` junto al use-case.

**No** inyectes `PRISMA_SERVICE` en application — solo en infrastructure.

### Registrar en producto

`apps/clientes/josanz/backend/src/app/app.module.ts` importa `WidgetsModule` si el producto lo expone. Otros clientes eligen en su propio `AppModule`.

---

## Frontend kernel

Cuatro capas bajo `libs/base/frontend/angular/{domain}/` o `react/{domain}/`:

```
api → data-access → features (layout/pages/components) ← shell
```

UI genérica en `@base/angular-ui` / `@base/react-shared`.

Plantillas Arquetipos: thin `shell` + `features` → `@base/{domain}-features` — [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md).

---

## Shared / DTOs

Contratos isomórficos en `libs/base/shared/`. Frontend `*-api` y backend DTOs deben alinearse.

**Validación (F59, cerrada):** preferir reglas sync compartidas en shared (FE+BE)
frente a `Validators.*` / checks Nest duplicados — ver
[ADR 0012](../adr/adr-0012-isomorphic-validation.md).
Async (unicidad DB, etc.) queda en puerto BE + código de error estable para el FE.

---

## Verificación

```bash
npx tsc -p libs/base/backend/tsconfig.lib.json --noEmit
npx jest --config libs/base/backend/jest.config.ts
node tools/checks/check-frontend-conventions.mjs
```

Module boundaries: tag `layer:base` en libs nuevas.

---

## Enlaces

- [adr-0001](../adr/adr-0001-hexagonal-architecture.md)
- [add-backend-domain.md](./add-backend-domain.md)
- [add-frontend-domain.md](./add-frontend-domain.md)
