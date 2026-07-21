# Module Federation — desarrollo local

Cuándo usarla: desarrollar host + remotes MF en Arquetipos (companion del
runbook de deploy).

Deploy: [module-federation-deploy.md](../runbooks/module-federation-deploy.md).

## Idea

Host Angular carga remotes React (u otros) en runtime. Shared store / API kernel
viven en paquetes `@base/*` crosscutting para no duplicar React/Angular state.

## Arranque local

Consulta scripts del root (`package.json`) — típicamente:

```bash
pnpm arq:fe:mf
# o nx serve de mf-host + remotes según project.json
```

Puertos habituales (confirmar en `project.json` / `local-development`):

| Pieza | Puerto orientativo |
|-------|--------------------|
| MF host | 4210 |
| Remote(s) | 4211, 4212… |

## Reglas

1. Remotes no importan `@josanz/*` desde plantillas arquetipos.
2. Contratos compartidos vía `@base/shared` / shared-store.
3. E2E smoke: Playwright en `mf-host-e2e` (login + mount de remote).

## Verificación

```bash
pnpm exec playwright test -c apps/arquetipos/frontend/angular/microfrontend/mf-host-e2e/playwright.config.mts --project=chromium
```

## Enlaces

- ADR [0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)
- [testing-pyramid.md](./testing-pyramid.md)
- [local-development.md](./local-development.md)
