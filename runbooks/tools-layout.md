<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Layout de `tools/` — utilidades del monorepo</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarlo: localizar un script de CI/DX, scaffold o gate de convenciones sin
buscar en un flat `tools/scripts/` (eliminado).

> Índice canónico en código: [`tools/README.md`](../../tools/README.md)  
> Agentes IA: [`tools/ai/README.md`](../../tools/ai/README.md) · [`.opencode/README.md`](../../.opencode/README.md)

---

## Mapa por carpeta

| Carpeta | Rol | Ejemplos `pnpm` / node |
|---------|-----|-------------------------|
| `checks/` | Gates de convención y paridad | `pnpm check:lib-layout`, `pnpm hygiene` |
| `dx/` | Experiencia de desarrollo | `pnpm kill:node`, `pnpm josanz-api:dev`, `pnpm sync:workspace-deps` |
| `jest/` | Preset Jest + coverage merge | `pnpm check:jest-preset`, `pnpm test:coverage:merge` |
| `typecheck/` | Plugin Nx + walkers legacy | `pnpm typecheck:all`, `pnpm typecheck:all:legacy` |
| `lib/` | Helpers compartidos (`run-tsc`, imports) | usado por el plugin typecheck |
| `scaffolds/` | Generadores dominio / app / cliente | ver [guides/scaffold-arquetipos.md](../guides/scaffold-arquetipos.md) · [scaffolds README](../../tools/scaffolds/README.md) · plan [F57-B1](../plans/rounds/plans-57-fifty-seven-round/1750000142000-f57-scaffolds-unified-cli.md) |
| `seeds/` | Seeds demo + Keycloak CI | `pnpm josanz:seed-demo`, `pnpm keycloak:seed` |
| `smoke/` | Smokes backend / SaaS / doc-gen | `pnpm test:smoke:backend`, `pnpm saas:verifactu:smoke` |
| `migrate/` | Codemods / pilots one-off | `pnpm pilot:remove-paths:list` |
| `publish/` | Pack / publish libs canario | `pnpm pack:publishable` |
| `mockserver/` | FE sin API (OIDC + fixtures) | `pnpm mockserver` — [mockserver.md](./mockserver.md) |
| `metro/` | Config Expo / Metro | apps React Native |
| `ai/` | Migraciones Nx AI + índice agentes | no ejecutable; docs |
| `archive/` | Solo README — basura de raíz ya eliminada | no CI |

Prefer **`pnpm <script>`** del `package.json` raíz frente a invocar `node tools/…` a mano.

---

## Renombre (histórico)

| Antes | Ahora |
|-------|--------|
| `tools/scripts/*.mjs` | `tools/{checks,dx,jest,…}/` según utilidad |
| `tools/scripts/lib/` | `tools/lib/` |
| `tools/ai-migrations/` | `tools/ai/migrations/` |

Los planes en `docs/plans/rounds/` pueden citar paths viejos; **gana esta biblia** y
`tools/README.md`.

---

## Gates habituales (CI / PR)

```bash
pnpm verify:affected          # lint + typecheck + test (affected)
pnpm hygiene                  # layout + paths + deps + barrels + native-first
pnpm check:lib-layout:strict
pnpm check:exports-paths
pnpm check:jest-preset:strict
pnpm check:arquetipos-parity -- --strict
```

Detalle typecheck/lint: [typecheck-and-lint-gates.md](./typecheck-and-lint-gates.md).  
Coverage Jest: [jest-coverage.md](./jest-coverage.md).  
Frontend CI: [frontend/ci-gates.md](../frontend/ci-gates.md).

---

## Enlaces

- [docs/README.md](../README.md) — hub
- [guides/README.md](../guides/README.md) — tabla de scaffolds
- [AGENTS.md](../../AGENTS.md) — contrato agentes
