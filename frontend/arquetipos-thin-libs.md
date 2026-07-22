<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Arquetipos — domain libs (plantillas con override)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

**ID plan:** F7-S4.arquetipos-docs (actualizado: 4 capas con re-exports)  
**Ámbito:** `libs/arquetipos/frontend/{angular,react,next}/` y
`libs/arquetipos/frontend/mobile/{ionic,rn}/`

Las apps plantilla (`apps/arquetipos/frontend/*`) demuestran cómo cablear dominios
sobre `@base/*` **sin duplicar** la implementación por defecto. Cada dominio
expone las **4 capas** (`api`, `data-access`, `shell`, `features`) para que un
producto o la propia plantilla pueda **sobrescribir** contratos o data-access
sin tocar base. El default es un **re-export thin** de `@base/*`.

---

## Regla principal

| Capa | ¿Existe en arquetipos? | Default | Override |
|------|------------------------|---------|----------|
| `api` | **Sí** | re-export `@base/{domain}-api` (React: `@base/react-{domain}-api`) | Sustituir exports / añadir DTOs de plantilla |
| `data-access` | **Sí** | re-export `@base/{domain}-data-access` (React: `@base/react-{domain}-…`) | Envolver stores/servicios |
| `features` | **Sí** | UI plantilla + `layout/` `pages/` `components/` | Branding / páginas propias |
| `shell` | **Sí** | rutas lazy → features | Entry de dominio |
| `ui` | **Sí** | presentacional plantilla | `@arquetipos/arquetipos-{angular,react}-ui` |

**No omitir** `api/` ni `data-access/` en dominios de plantilla: sin esas carpetas
no hay punto de extensión. Mientras no haga falta lógica propia, el `index.ts`
puede ser solo `export * from '@base/…'`.

El linter `check-lib-layout.mjs` espera **4 capas** en
`libs/arquetipos/frontend/{angular|react}/{domain}/`.

---

## Árbol físico

```
libs/arquetipos/frontend/
├── angular/
│   ├── ui/                    # @arquetipos/arquetipos-angular-ui
│   ├── src/                   # @arquetipos/angular (kernel/barrel)
│   └── {domain}/              # auth, clients, users, audit, roles, …
│       ├── api/               # @arquetipos/angular-{domain}-api
│       ├── data-access/       # @arquetipos/angular-{domain}-data-access
│       ├── shell/             # @arquetipos/angular-{domain}-shell
│       └── features/          # @arquetipos/angular-{domain}-features
│           └── src/
│               ├── layout/
│               ├── pages/
│               ├── components/
│               ├── {domain}.routes.ts
│               └── index.ts
└── react/
    ├── ui/                    # @arquetipos/arquetipos-react-ui
    ├── src/                   # @arquetipos/browser-react (kernel/barrel)
    └── {domain}/
        ├── api/
        ├── data-access/
        ├── shell/
        └── features/
            └── src/{layout,pages,components,…}
```

---

## Paquetes por dominio (Angular)

| Dominio | api | data-access | shell | features | Base (default) |
|---------|-----|-------------|-------|----------|----------------|
| auth | `@arquetipos/angular-auth-api` | `@arquetipos/angular-auth-data-access` | `…-shell` | `…-features` | `@base/auth-*` |
| clients | `@arquetipos/angular-clients-api` | `@arquetipos/angular-clients-data-access` | … | … | `@base/clients-*` |
| users | `@arquetipos/angular-users-api` | `@arquetipos/angular-users-data-access` | … | … | `@base/users-*` |
| audit | `@arquetipos/angular-audit-api` | `@arquetipos/angular-audit-data-access` | … | … | `@base/audit-*` |
| roles | `@arquetipos/angular-roles-api` | `@arquetipos/angular-roles-data-access` | … | … | `@base/roles-*` |

### Patrón api / data-access (thin)

```typescript
// libs/arquetipos/frontend/angular/clients/api/src/index.ts
/** Thin re-export — override here to specialize the arquetipos template. */
export * from '@base/clients-api';

// libs/arquetipos/frontend/angular/clients/data-access/src/index.ts
export * from '@base/clients-data-access';
```

### Cómo sobrescribir un método (demo clients)

No hace falta sustituir todo el data-access: **extiende la clase base**, override un
método, y elige implementación con flag/token.

| Pieza | Path | Rol |
|-------|------|-----|
| Base method | `libs/base/.../clients/data-access/.../clients.service.ts` → `sumar(a,b)` | Demo educational (`a+b`); no es API HTTP |
| Override class | `libs/arquetipos/.../clients/data-access/src/arquetipos-clients.service.ts` | `extends ClientsService`, `override sumar` → `a+b+1` |
| DI switch | `provideClientsDataAccess()` | Si mode=`override`, `{ provide: ClientsService, useClass: ArquetiposClientsService }` |
| Flag | `ARQ_CLIENTS_DATA_ACCESS` / `NG_APP_…` / `VITE_…` | `'base'` (default) \| `'override'` |
| Load extra | `ArquetiposClientsService.load` | En override, prefija nombres con `[ARQ]` |
| UI hint | features panel | Badge `override · sumar(2,3)=6` solo en override |

**Pasos para otro método:**

1. Añadir o elegir el método en `@base/{domain}-data-access`.
2. En arquetipos, clase que `extends` el servicio base y hace `override metodo(...)`.
3. En `provide…DataAccess()`, si mode=`override`: `useClass` de la clase plantilla
   como token del servicio base.
4. Features importan el token base desde `@arquetipos/…-data-access` (no `@base/…`
   directo) para que el override propague.
5. Apps registran `provideClientsDataAccess()` en `app.config` / entry (sin args
   basta para leer env).

**Flip base ↔ override**

```bash
# Override (Angular)
NG_APP_ARQ_CLIENTS_DATA_ACCESS=override pnpm nx serve angular-single

# Override (React / Vite)
VITE_ARQ_CLIENTS_DATA_ACCESS=override pnpm nx serve react-single

# Or pin in code
provideClientsDataAccess({ mode: 'override' })  // Angular app.config
provideClientsDataAccess({ mode: 'override' })  // React main.tsx
```

Default is **`base`** — apps keep base `sumar` / unprefixed names until you flip.

**React (simétrico):** `sumar` en `@base/react-clients-data-access`; arquetipos
reexporta un `sumar` que suma `+1` cuando mode=`override`. No hay DI de clases;
features importan `sumar` desde `@arquetipos/react-clients-data-access`.

### Features — layout / pages / components

```typescript
// features/src/clients.routes.ts — layout → page → components
export const clientsFeatureRoutes: Route[] = [
  {
    path: '',
    loadComponent: () =>
      import('./layout/clients-feature.layout').then((m) => m.ClientsFeatureLayout),
    children: [
      {
        path: '',
        loadComponent: () =>
          import('./pages/arquetipos-clients.page').then((m) => m.ArquetiposClientsPageComponent),
      },
    ],
  },
];
```

Las features deben preferir `@arquetipos/angular-{domain}-data-access` (no
`@base/…-data-access` directo) para que un override en arquetipos propague.

---

## Paquetes por dominio (React)

| Dominio | api | data-access | Base (default) |
|---------|-----|-------------|----------------|
| auth / clients / users / audit / roles | `@arquetipos/react-{domain}-api` | `@arquetipos/react-{domain}-data-access` | `@base/react-{domain}-*` |
| settings | `@arquetipos/react-settings-api` | `@arquetipos/react-settings-data-access` | `@base/shared` (sin lib React settings en base aún) |

---

## Apps plantilla — wiring

| App | Rutas dominio |
|-----|----------------|
| `angular-single`, `angular-multi` | `loadChildren` → `@arquetipos/angular-{domain}-shell` |
| `react-single`, `react-multi` | `React.lazy` → `@arquetipos/react-{domain}-shell` |
| `next-single`, `next-multi` | pages → `@arquetipos/next-{domain}-features` |
| `ionic-single`, `ionic-multi` | `loadChildren` → `@arquetipos/ionic-{domain}-shell` |
| `react-native-single`, `react-native-multi` | App monta `@arquetipos/react-native-{domain}-features` |

---

## Next / Ionic / React Native (F35 mobile+next layer split)

Mismo patrón thin que Angular/React web: arquetipos re-exporta `@base/*`.

| Stack | UI plantilla | Dominios (api/data-access/shell/features) | Base |
|-------|--------------|------------------------------------------|------|
| Next | `@arquetipos/arquetipos-next-ui` | `@arquetipos/next-{clients\|users}-*` | `@base/next-*` |
| Ionic | `@arquetipos/arquetipos-ionic-ui` | `@arquetipos/ionic-{auth\|clients}-*` | `@base/ionic-*` |
| React Native | `@arquetipos/arquetipos-react-native-ui` | `@arquetipos/react-native-{clients\|users}-*` | `@base/react-native-*` |

Árbol físico:

```
libs/base/frontend/
  next/…                          → @base/next-*
  mobile/react-native/…           → @base/react-native-*
  mobile/ionic/…                  → @base/ionic-*

libs/arquetipos/
  frontend/next/{ui,clients,users}/
  mobile/ionic/{ui,auth,clients}/
  mobile/react-native/{ui,clients,users}/
```

---

## Barrels y UI

| Paquete | Path | Rol |
|---------|------|-----|
| `@arquetipos/angular` | `frontend/angular/src` | Kernel/barrel Angular |
| `@arquetipos/browser-react` | `frontend/react/src` | Kernel/barrel React |
| `@arquetipos/arquetipos-angular-ui` | `frontend/angular/ui` | UI plantilla Angular |
| `@arquetipos/arquetipos-react-ui` | `frontend/react/ui` | UI plantilla React |
| `@arquetipos/arquetipos-next-ui` | `frontend/next/ui` | UI plantilla Next |
| `@arquetipos/arquetipos-ionic-ui` | `mobile/ionic/ui` | UI plantilla Ionic |
| `@arquetipos/arquetipos-react-native-ui` | `mobile/react-native/ui` | UI plantilla RN |

---

## Al copiar a un producto cliente

1. Las 4 capas de arquetipos son el **molde** de override; si el producto necesita
   reglas propias, crear `@josanz/{domain}-{api,data-access,shell,features}` (u otro
   scope) en lugar de depender de la plantilla a largo plazo.
2. Empezar sustituyendo solo `data-access` / `features` y dejar `api` como
   re-export hasta que los contratos diverjan.

---

## Verificación

```bash
node tools/checks/check-lib-layout.mjs
node tools/checks/check-frontend-conventions.mjs
pnpm nx typecheck arquetipos-angular-clients-features
pnpm nx typecheck arquetipos-react-clients-features
```

---

## Enlaces

- [docs/README.md](../README.md)
- [AGENTS.md](../../AGENTS.md)
- [ui-component-catalog.yaml](./ui-component-catalog.yaml)
