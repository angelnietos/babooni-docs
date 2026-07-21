# F56-C2 — Cablear FE arquetipos al MockServer (sin API)

## Estado

completado

## Objetivo

FE plantilla apunta a mock vía proxy + OIDC URL, sin gateway ni Keycloak.

## Criterios de aceptación

- [x] Default sigue gateway `:4000` / Keycloak `:9080`.
- [x] Con flag/script mock, FE single puede login → clients sin API Nest.
- [x] Docs + scripts root.

## Resultado

- Angular: `serve --configuration=mock`, `proxy.conf.mock.json`,
  `environment.mock.ts` → `provideKeycloakConfig({ url: mock })`.
- React: `API_PROXY_TARGET` + `VITE_KEYCLOAK_URL` vía
  `tools/scripts/serve-react-single-mock.mjs`.
- Scripts: `arq:fe:angular-single:mock`, `arq:fe:react-single:mock`.
