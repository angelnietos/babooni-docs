# F56-C2 — Cablear FE arquetipos al MockServer (sin API)

## Estado

listo para ejecutar

## Objetivo

Que un developer front pueda:

```bash
pnpm mockserver          # :4010
pnpm arq:fe:angular-single:mock   # o USE_MOCKSERVER=1 nx serve …
```

…y la SPA hable con el mock vía proxy `/api` **sin** api-gateway `:4000` ni
Keycloak (login stub).

## Prerrequisitos

- F56-C1 MockServer usable.
- Proxies actuales: Angular `proxy.conf.json` → `:4000`; React Vite `apiProxy`.

## Tareas

1. Unificar target proxy vía env, p. ej. `API_PROXY_TARGET` default
   `http://localhost:4000`, mock `http://localhost:4010`.
2. Angular: `proxy.conf.json` generado o `proxy.conf.mock.json` +
   `serve` configuration `mock` en `project.json`.
3. React Vite: leer `process.env.API_PROXY_TARGET` en `vite.config.mts`.
4. Scripts root:
   - `pnpm arq:fe:angular-single:mock`
   - `pnpm arq:fe:react-single:mock`
   - opcional `pnpm arq:fe:mock` (concurrently mock + FE)
5. Documentar en local-development + runbook mockserver.
6. Auth: modo mock debe evitar redirect a Keycloak real (stub en mock o flag
   `AUTH_MODE=mock` en bootstrap FE) — detallar en Resultado.
7. No romper el flujo “API real” por defecto.

## Criterios de aceptación

- [ ] Default sigue siendo gateway `:4000`.
- [ ] Con flag/script mock, FE single carga login → clients **sin** API Nest.
- [ ] Docs + scripts root. Multi-tenant opcional en la misma ronda.
