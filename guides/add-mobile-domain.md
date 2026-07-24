<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Añadir dominio móvil (Ionic / React Native)</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: nuevas pantallas o dominios en apps bajo `apps/arquetipos/mobile/`
o libs `libs/{base,arquetipos}/frontend/mobile/`.

Default de plataforma (ADR 0008): **SPA web + monolito**. Mobile es **opt-in**.

## Mapa de apps

| App Nx | Puerto web | Rol |
|--------|------------|-----|
| `ionic-single` | 4300 | Ionic single-tenant |
| `ionic-multi` | 4301 | Ionic multi-tenant (switcher) |
| `react-native-single` | 8091 | Expo RN web single |
| `react-native-multi` | 8092 | Expo RN web multi |

```bash
pnpm nx serve ionic-single
pnpm nx serve react-native-single   # Expo --web
```

## Capas RN (patrón thin)

```
@arquetipos/react-native-{domain}-features  →  @base/react-native-{domain}-features
@arquetipos/arquetipos-react-native-ui      →  @base/react-native-ui
```

Paths: `libs/base/frontend/mobile/rn/` y `libs/arquetipos/frontend/mobile/rn/`.
Detalle de paths: [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md).

## Data-access facade (F65-D1) + panel SoC (F66-A1)

Ionic / RN clients features consumen un **puerto** en data-access
(`IonicClientsFacade`, `useClientsFacade`) — no arrays demo mutables ni list
state ad-hoc en la página. Guía: [state-soc-facade.md](../frontend/state-soc-facade.md).

Features: `layout/` · `pages/` · `components/` (+ `services/` / `hooks/` para
orquestación) — [features-layout-soc.md](../frontend/features-layout-soc.md).
Vistas read/write: [entity-view-abstractions.md](../frontend/entity-view-abstractions.md).

## Auth demo

`AuthApp` (`@base/react-native-auth-features`) monta login + slots `clients` / `users`:

```tsx
<AuthApp
  loginTitle="Arquetipos"
  clients={<ClientsFeature />}
  users={<UsersFeature />}
/>
```

Multi añade `loginChildren` (switcher de tenant) y `brandColor` por tenant.

## Temas tenant + atmósfera (Ionic multi)

Misma API que Angular/React (`@base/shared`):

1. Bootstrap: `applyArqDemoTenant()` **antes** de `applyArqVisualTheme()`.
2. Settings (`@base/ionic-settings-features`): cards de identidad (5 tenants) +
   atmósfera Switch/Connect/Studio + RBAC — paridad web.
3. CSS global en `ionic-multi` `styles[]`: `tenants.css` +
   `arq-atmosphere-overrides.css` (después de `variables.css`).
4. Con tenant activo, atmósfera solo cambia radios; la marca (nav / CTA / canvas)
   la pone el tenant (inline en `<html>` + `[data-arq-tenant]`).

Checklist visual: [tenant-themes-checklist.md](../frontend/tenant-themes-checklist.md).

Demo login: `admin@demo.com` / `admin` (también manager / member).

## Metro — React único (apps Expo)

El monorepo hoist **React 19** en la raíz; las apps Expo RN usan **React 18**.
Eso **ya está resuelto** en las plantillas: Metro pinnea un solo árbol de React
desde `node_modules` de la app. No es un bug abierto ni una pantalla en blanco
“pendiente”.

Paquete workspace: `@arquetipos/expo-metro-config`
(`libs/arquetipos/frontend/mobile/rn/metro-config`). Cada app declara
`workspace:*` y requiere por nombre:

```js
// apps/arquetipos/mobile/react-native-{single,multi}/metro.config.js
const {
  createArquetiposExpoMetroConfig,
} = require('@arquetipos/expo-metro-config');
module.exports = createArquetiposExpoMetroConfig(__dirname);
```

El helper fija:

- `disableHierarchicalLookup: true`
- `extraNodeModules` → `react`, `react-dom`, `react-native`, `react-native-web`
  (y peers) desde `node_modules` de la **app**

Si al tocar Metro aparece pantalla en blanco o `$$typeof` / *Objects are not
valid as a React child*: restaurar el helper y reiniciar Expo con `--clear`.

## Peers canónicos (F52-C1)

| Paquete | Peers |
|---------|--------|
| `@base/angular-ui` | `@angular/common\|core\|router` ^21, `rxjs` ^7.8 |
| `@base/ionic-ui` | `@angular/common\|core` ~21.2, `@ionic/angular` ^8.5 |
| `@base/react-native-ui` | `react`/`react-dom` **18.3.1**, `react-native` 0.76.9, `react-native-web` ~0.19 |
| Apps RN Expo | React **18** (no 19 del hoist raíz) — ver Metro arriba |

Frameworks van en **peers**, no en `dependencies` de las libs UI (evita dual package).
Publicables: [npm-publish-and-versioning.md](./npm-publish-and-versioning.md).

## Typecheck

```bash
pnpm nx typecheck react-native-single
# o tsc -p apps/arquetipos/mobile/react-native-single/tsconfig.json --noEmit
```

Mismatch `@types/react` 18 vs 19: ver nota en [local-development.md](./local-development.md).

## Verificación

```bash
pnpm nx serve react-native-single
# Abrir http://localhost:8091 — login demo (Metro pin ya incluido en la plantilla)
node tools/checks/check-lib-layout.mjs
```

## Enlaces

- ADR [0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)
- [local-development.md](./local-development.md) — matriz de puertos
- [testing-pyramid.md](./testing-pyramid.md)
