# MockServer — desarrollo FE sin API (F56)

Ver guía corta en [`tools/mockserver/README.md`](../../tools/mockserver/README.md).

## Por qué

Los fronts de plantilla (`angular-single`, `react-single`) proxyan `/api` al
gateway (`:4000`) y autentican contra Keycloak (`:9080`). Eso obliga a
infra + Nest para UI. El MockServer concentra **OIDC stub + fixtures `/api/*`**
en un solo proceso Node (`:4010`).

## Comandos

```bash
pnpm mockserver                     # :4010
pnpm arq:fe:angular-single:mock     # serve Angular + proxy/Keycloak → mock
pnpm arq:fe:react-single:mock       # serve React + env mock
pnpm arq:fe:build:smoke             # build canario angular-single + react-single
```

## Variables

| Var | Default | Uso |
|-----|---------|-----|
| `MOCKSERVER_PORT` | `4010` | Puerto del mock |
| `API_PROXY_TARGET` | `http://localhost:4000` | Target proxy Vite `/api` |
| `VITE_KEYCLOAK_URL` | `http://localhost:9080` | React OIDC base |
| Angular `environment.useMockServer` | `false` | Activo en config `serve:mock` |

## Checklist visual (F56-A2)

Con mock levantado:

1. Abrir login — demos clicables; no debe decir “identidad no responde”.
2. Login `demo@demo.com` → `/clients` con filas mock (Acme / Beta).
3. Nav Users / Audit cargan sin pantalla en blanco.
4. Readonly: mismos datos; RBAC nav según JWT (sin Audit write).

## Relación CI

Build smoke (`arq:fe:build:smoke`) es independiente del mock. E2E con mock es
opcional/soft; el job `e2e-web` en main sigue usando Keycloak real.
