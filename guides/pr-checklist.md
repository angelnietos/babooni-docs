<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Checklist de PR</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: antes de abrir o pedir review de un PR en este monorepo.

Mapa CI ↔ local: [frontend/ci-gates.md](../frontend/ci-gates.md) ·
[runbooks/typecheck-and-lint-gates.md](../runbooks/typecheck-and-lint-gates.md) ·
`.github/workflows/ci.yml`.

## Código

- [ ] Cambio en **lib** correcta (`@base` vs producto vs SaaS) — sin cruzar capas npm
- [ ] Slug dominio FE = BE = ruta `/api/{slug}` si aplica
- [ ] UI: re-export vs wrapper evaluado ([ui-re-export-vs-wrapper.md](./ui-re-export-vs-wrapper.md))
- [ ] Handlers sin Prisma; tests unit de handlers nuevos ([testing-pyramid.md](./testing-pyramid.md))

## Verificación local (espejo CI)

Mínimo antes de pedir review:

```bash
pnpm verify:affected          # lint + typecheck + test (job verify / frontend)
```

Gates que CI corre además (alinear con el alcance del PR):

```bash
pnpm check:schema-parity              # Prisma / schema
pnpm check:domain-conventions:strict  # backend hex
pnpm check:frontend-conventions       # FE
pnpm check:lib-layout:strict
pnpm check:legacy-paths
pnpm check:node-modules
pnpm check:lint-coverage
pnpm check:slim-barrel
pnpm check:tsconfig-paths && pnpm check:exports-paths
pnpm check:workspace-deps:strict
pnpm check:ui-ownership:strict        # si UI / features
pnpm check:deprecated
```

En **push a `main`**, CI añade `pnpm typecheck:all` (red de seguridad F16-TC).
En PRs basta `verify:affected` + los `check:*` del job.

Si Nx cuelga: [nx-daemon.md](../runbooks/nx-daemon.md) o `tsc` / jest directo.
Coverage: [jest-coverage.md](../runbooks/jest-coverage.md) ·
`pnpm test:coverage:report:affected` (opcional).

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
- [frontend/ci-gates.md](../frontend/ci-gates.md)
- [runbooks/typecheck-and-lint-gates.md](../runbooks/typecheck-and-lint-gates.md)
