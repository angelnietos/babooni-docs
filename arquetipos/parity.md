# Paridad arquetipos (F54-B4)

Las plantillas `apps/arquetipos/**` + libs `@arquetipos/*` deben ser **lo más
parecidas posible** en tres ejes:

| Eje | Contrato | Tooling |
|-----|----------|---------|
| **Arquitectura** | 4 capas por dominio (`api` → `data-access` → `features` ← `ui`), shells lazy, apps thin, thin → `@base/*` | `check:arquetipos-parity` + `check:lib-layout` |
| **Visual** | SoT `@base/native-ui` + **7–1** `@base/ui-styles` (`arq-auth*` / `arq-clients*` BEM; F62 amplía users/roles/shell). Temas tenant **distinguibles**. Sin `<select>` OS en features. RN: tokens. | `check:ui-native-first` + revisión visual F62 |
| **Funcional** | Mismos flujos demo / rutas canario | manifest `required_routes` + dual-stack clients |

## Manifest

Fuente de verdad: [parity.manifest.yaml](./parity.manifest.yaml).

## Checker

```bash
pnpm check:arquetipos-parity -- --strict   # CI frontend (F55-C2)
pnpm check:arquetipos-parity               # soft local
```

## Niveles por stack

| Nivel | Stacks | Exigencia |
|-------|--------|-----------|
| Full | Angular / React SPA | 4 capas + UI native + rutas |
| Core | Next | Subset |
| Core (mobile) | Ionic / RN | Mismas rutas canónicas `TEMPLATE_NAV`; Ionic desktop = shell SPA; viewport móvil = `@base/mobile-chrome` / `ArqMobileShell` |
| Token | RN (histórico) | Sustituido por Core (mobile) en F63-C1 |

## Visual / layout smoke (F55-B2 / F56-A2 / F58-D2)

Secuencia canario **idéntica** en `angular-single-e2e` y `react-single-e2e`
(`src/smoke.spec.ts`):

1. Unauthenticated `/clients` → redirect `/auth/login`
2. Login demo → `/clients`, `/users`, `/audit` con `main` visible
3. Login readonly → sin link Audit; Clients visible

**Sin API (F56 / F58-D1):** `pnpm mockserver` + `pnpm arq:fe:angular-single:mock` (o
`react-single:mock`) — checklist + `pnpm mockserver:e2e:soft` en
[mockserver.md](../runbooks/mockserver.md).

**Build:** `pnpm arq:fe:build:smoke` (CI soft).

### Matriz e2e Playwright (F58-D2)

| Proyecto | Puerto FE | Comando | CI `e2e-web` (main) | Auth |
|----------|-----------|---------|--------------------|------|
| `angular-single-e2e` | 4200 | `pnpm nx run angular-single-e2e:e2e -- --project=chromium` | sí | Keycloak |
| `react-single-e2e` | 4201 | `pnpm nx run react-single-e2e:e2e -- --project=chromium` | sí | Keycloak |
| `react-multi-e2e` | 4202 | `pnpm nx run react-multi-e2e:e2e -- --project=chromium` | sí | Keycloak |
| `angular-multi-e2e` | 4203 | `pnpm nx run angular-multi-e2e:e2e -- --project=chromium` | sí | Keycloak |
| `mf-host-e2e` | 4210 (+4211/4212) | `pnpm nx run mf-host-e2e:e2e -- --project=chromium` | **soft** | Keycloak |
| `next-single-e2e` | 4240 | `pnpm nx run next-single-e2e:e2e -- --project=chromium` | soft / local | cookie demo |
| `next-multi-e2e` | 4241 | `pnpm nx run next-multi-e2e:e2e -- --project=chromium` | soft / local | cookie demo |
| `ionic-single-e2e` | 4300 | `pnpm nx run ionic-single-e2e:e2e -- --project=chromium` | soft / local | AuthSession demo |
| `ionic-multi-e2e` | 4301 | `pnpm nx run ionic-multi-e2e:e2e -- --project=chromium` | soft / local | AuthSession demo |
| `react-native-single-e2e` | 8091 (Expo web) | `pnpm nx run react-native-single-e2e:e2e -- --project=chromium` | soft / local | demoLogin |
| `react-native-multi-e2e` | 8092 (Expo web) | `pnpm nx run react-native-multi-e2e:e2e -- --project=chromium` | soft / local | demoLogin |

Local canónico SPA (Keycloak `:9080` seed):

```bash
pnpm arq:fe:e2e:smoke
```

Next + mobile (sin Keycloak; auth demo local):

```bash
pnpm arq:fe:e2e:next
pnpm arq:mobile:e2e:smoke
# o todo: pnpm arq:e2e:all
pnpm arq:typecheck:mobile-next
pnpm arq:fe:build:smoke   # incluye next + ionic build
```

Chromatic / Code Connect → carry **F62-D2**
([plans-62](../plans/rounds/plans-62-sixty-two-round/1750000187000-f62-carry-visual-tooling-deprecated.md)).
Composiciones nativas login/clients → **F60-C1/C2/D1**.
