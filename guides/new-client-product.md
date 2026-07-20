# Nuevo producto cliente

Resumen ejecutivo. Checklist completo: [clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md).

---

## Idea

Un producto cliente es una **copia estructurada de Josanz**, no de Arquetipos:

- Vive en `apps/clientes/{slug}/` + `libs/clientes/{slug}/`
- Scope npm `@acme/*` (ejemplo)
- Importa **`@base/*` únicamente** — nunca `@arquetipos/*`

**Por qué:** Arquetipos son plantillas de demostración; un cliente en producción no debe depender de ellas.

---

## Árbol mínimo

```
libs/clientes/acme/
├── shared/
├── backend/
├── angular-ui/
└── angular/
    ├── platform/
    └── {domain}/

apps/clientes/acme/
├── backend/     → acme-api
└── frontend/acme/
```

---

## Flujo rápido

```bash
# 1. Scaffold libs
node tools/scripts/scaffold-cliente-product.mjs --slug acme --scope acme

# 2. Copiar apps desde Josanz y renombrar (josanz → acme)
# 3. tsconfig.base.json paths @acme/*
# 4. pnpm install
# 5. Primer dominio
node tools/scripts/scaffold-josanz-domain.mjs orders "Pedidos"
# (mover/adaptar paths a acme si el script aún genera josanz)

# 6. Verificar
node tools/scripts/check-lib-layout.mjs --strict
pnpm nx serve acme-api
pnpm nx serve acme
```

---

## Qué copiar de dónde

| Necesidad | Fuente |
|-----------|--------|
| Dominios kernel | `@base/*` directo |
| Wiring monolito backend | `apps/clientes/josanz/backend/` |
| Wiring SPA | `apps/clientes/josanz/frontend/josanz/` |
| Cómo enlazar rutas thin | [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md) (solo como referencia de routing, no copiar libs) |

---

## Bootstrap de producto

Crea `bootstrap-env.ts` en el backend del cliente:

```typescript
import { applyProductDatabaseUrl } from '@base/backend';

process.env.TENANT_MODE = process.env.TENANT_MODE ?? 'single';

applyProductDatabaseUrl({
  keys: ['ACME_DATABASE_URL', 'DATABASE_URL'],
  label: 'Acme ERP',
});
```

---

## Enlaces

- [nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md)
- [architecture/overview.md](../architecture/overview.md)
