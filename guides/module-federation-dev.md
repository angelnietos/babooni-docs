<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Module Federation — desarrollo local</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

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
