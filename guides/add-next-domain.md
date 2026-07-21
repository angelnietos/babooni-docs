# Añadir dominio Next.js

Cuándo usarla: pantallas o dominios en `apps/arquetipos/frontend/nextjs/`
(`next-single`, `next-multi`) o libs Next en `libs/base/frontend/next/`.

Default plataforma (ADR 0008): SPA. Next es **opt-in** (SSR/SEO).

## Apps

| App | Puerto típico | Rol |
|-----|---------------|-----|
| `next-single` | 4240 | Plantilla single-tenant |
| `next-multi` | (ver project) | Plantilla multi-tenant |

```bash
pnpm nx serve next-single
```

## Capas vs SPA

Misma idea de 4 capas, pero el **App Router** de Next es el composition root:

| Capa | Rol en Next |
|------|-------------|
| `*-api` / shared | Contratos |
| data-access | Fetch server/client según límite SSR |
| features / pages | UI de ruta |
| `*-ui` | Presentacional (`@base/next-ui` o thin arquetipos) |

**No** mezclar imports `@josanz/*` en plantillas arquetipos. Thin → `@base/*`:
[arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md).

## Boundaries SSR

- Datos sensibles / session: preferir Server Components o route handlers.
- Stores de cliente (auth bootstrap): no bloquear el árbol entero sin timeout
  (evitar “Cargando…” infinito).
- `paths` en `tsconfig.base.json` deben cubrir cada `exports` del paquete FE
  (`pnpm check:exports-paths`).

## Auth

Alinear con Keycloak del producto/plantilla ([keycloak-setup.md](./keycloak-setup.md)).
Login demo debe navegar tras submit (no no-op).

## Verificación

```bash
pnpm nx serve next-single
pnpm check:exports-paths
pnpm nx typecheck next-single
```

## Enlaces

- ADR [0006](../adr/adr-0006-frontend-layering.md), [0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)
- [workspace-packages.md](../frontend/workspace-packages.md)
