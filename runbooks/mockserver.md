# MockServer — desarrollo FE sin API (F56)

Cuándo usarlo: prototipar login + listados de plantillas (`angular-single` /
`react-single`) **sin** Nest, gateway ni Keycloak Docker.

Implementación: [`tools/mockserver/`](../../tools/mockserver/README.md).
Mapa utilidades: [tools-layout.md](./tools-layout.md).

## Por qué

Los fronts de plantilla (`angular-single`, `react-single`) proxyan `/api` al
gateway (`:4000`) y autentican contra Keycloak (`:9080`). Eso obliga a
infra + Nest para UI. El MockServer concentra **OIDC stub + fixtures `/api/*`**
en un solo proceso Node (`:4010`).

## Comandos

```bash
pnpm mockserver                     # :4010
pnpm mockserver:smoke               # health + token + clients + settings
pnpm mockserver:e2e:soft            # F58-D1 soft API flow (no Keycloak)
pnpm arq:fe:angular-single:mock     # serve Angular + proxy/Keycloak → mock
pnpm arq:fe:react-single:mock       # serve React + env mock
pnpm arq:fe:build:smoke             # build canario angular-single + react-single
pnpm arq:fe:e2e:smoke               # F58-D2 Keycloak Playwright (todas las plantillas)
```

## Variables

| Var | Default | Uso |
|-----|---------|-----|
| `MOCKSERVER_PORT` | `4010` | Puerto del mock |
| `API_PROXY_TARGET` | `http://localhost:4000` | Target proxy Vite `/api` |
| `VITE_KEYCLOAK_URL` | `http://localhost:9080` | React OIDC base |
| Angular `environment.useMockServer` | `false` | Activo en config `serve:mock` |

## Checklist visual (F56-A2 / F58-D1)

Con mock levantado:

1. Abrir login — demos clicables; no debe decir “identidad no responde”.
2. Login `demo@demo.com` → `/clients` con filas mock (Acme / Beta).
3. Nav Users / Audit / Roles cargan sin pantalla en blanco.
4. Settings (si la app lo monta) responde `/api/settings` mock.
5. Readonly: mismos datos; RBAC nav según JWT (sin Audit write).

## Soft e2e API (F58-D1)

`pnpm mockserver:e2e:soft` arranca el mock en un puerto efímero y valida
login password-grant → `/api/auth/me` → clients/users/audit/roles/settings CRUD.
**No** arranca el SPA ni Keycloak Docker. Para el browser path usa la checklist
arriba; Keycloak real → job CI `e2e-web` / `pnpm arq:fe:e2e:smoke` ([parity.md](../arquetipos/parity.md)).

## Relación CI

Build smoke (`arq:fe:build:smoke`) es independiente del mock. Soft mock e2e es
local/`pnpm mockserver:e2e:soft` (no fail PR). El job `e2e-web` en main usa
Keycloak real + angular/react × single/multi (+ mf-host soft).
