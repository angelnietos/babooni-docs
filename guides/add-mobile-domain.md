# Añadir dominio móvil (Ionic / React Native)

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

## Metro — React único (crítico)

El monorepo hoist **React 19** en la raíz; Expo RN necesita **React 18**. Sin pin
en Metro, `ArqInput`/`ArqButton` fallan con *"Objects are not valid as a React child"*
(React duplicado).

Config compartida: `tools/metro/create-arquetipos-expo-metro-config.cjs`
(allowlist en `@nx/enforce-module-boundaries`, igual que configs ESLint).
Cada app:

```js
// apps/arquetipos/mobile/react-native-{single,multi}/metro.config.js
const {
  createArquetiposExpoMetroConfig,
} = require('../../../../tools/metro/create-arquetipos-expo-metro-config.cjs');
module.exports = createArquetiposExpoMetroConfig(__dirname);
```

El helper fija:

- `disableHierarchicalLookup: true`
- `extraNodeModules` → `react`, `react-dom`, `react-native`, `react-native-web`
  (y peers) desde `node_modules` de la **app**

Tras cambiar Metro: reiniciar Expo con `--clear`.

## Typecheck

```bash
pnpm nx typecheck react-native-single
# o tsc -p apps/arquetipos/mobile/react-native-single/tsconfig.json --noEmit
```

Mismatch `@types/react` 18 vs 19: ver nota en [local-development.md](./local-development.md).

## Verificación

```bash
pnpm nx serve react-native-single
# Abrir http://localhost:8091 — debe verse login (no pantalla blanca)
node tools/scripts/check-lib-layout.mjs
```

## Enlaces

- ADR [0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)
- [local-development.md](./local-development.md) — matriz de puertos
- [testing-pyramid.md](./testing-pyramid.md)
