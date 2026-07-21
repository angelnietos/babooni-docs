# Checklist de PR

Cuándo usarla: antes de abrir o pedir review de un PR en este monorepo.

## Código

- [ ] Cambio en **lib** correcta (`@base` vs producto vs SaaS) — sin cruzar capas npm
- [ ] Slug dominio FE = BE = ruta `/api/{slug}` si aplica
- [ ] UI: re-export vs wrapper evaluado ([ui-re-export-vs-wrapper.md](./ui-re-export-vs-wrapper.md))
- [ ] Handlers sin Prisma; tests unit de handlers nuevos ([testing-pyramid.md](./testing-pyramid.md))

## Verificación local

```bash
pnpm verify:affected          # lint + typecheck + test
pnpm check:lib-layout
pnpm check:frontend-conventions   # si FE
pnpm check:ui-ownership           # si UI
pnpm check:domain-conventions     # si backend hex
pnpm check:exports-paths          # si exports/paths FE
pnpm check:deprecated
```

Si Nx cuelga: [nx-daemon.md](../runbooks/nx-daemon.md) o `tsc` / jest directo.

## Docs

- [ ] Si cambió una convención: actualizar guía + índice ([CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md))
- [ ] Links relativos; sin paths de máquina

## Auth / datos (si aplica)

- [ ] Realm Keycloak alineado
- [ ] Migrate + paridad schema si tocaste Prisma (`pnpm check:schema-parity`)

## No hacer

- `git add -A` a ciegas (no meter `.env`, secrets, artefactos)
- Forzar push a `main`
- Saltar hooks sin petición explícita

## Enlaces

- [docs/README.md](../README.md) — sección Verificación
- [CONTRIBUTING.md](../../CONTRIBUTING.md)
