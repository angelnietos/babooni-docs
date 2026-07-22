# Extender un dominio kernel (`@base/*`)

Cuando el cambio beneficia a **todos** los productos (Josanz, plantillas, futuros clientes), va en el kernel — no en `@josanz/*`.

---

## ¿Kernel o producto?

| Pregunta | Si sí → kernel |
|----------|----------------|
| ¿Lo usaría un segundo cliente sin reglas Josanz? | `@base/backend` / `@base/angular-*` |
| ¿Es branding o regla comercial de Josanz? | `@josanz/*` |
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
