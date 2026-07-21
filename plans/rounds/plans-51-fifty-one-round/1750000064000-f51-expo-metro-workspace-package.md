# F51-C1 — Paquete `@arquetipos/expo-metro-config` (opcional)

## Estado

listo para ejecutar

## Objetivo

Hoy el helper vive en `tools/metro/…` con allowlist ESLint. Preferible un
paquete workspace `@arquetipos/expo-metro-config` importado por nombre
(sin relative fuera del project root ni allow especial).

## Dependencias

- **F51-A1** (pnpm install debe funcionar para actualizar lockfile).

## Tareas

1. Crear `libs/arquetipos/frontend/mobile/rn/metro-config` (CJS + `package.json`).
2. Deps `workspace:*` en `react-native-single` / `multi`.
3. Quitar allow de `tools/metro/…` en `eslint.config.mjs` si ya no se usa.
4. Actualizar [add-mobile-domain.md](../../../guides/add-mobile-domain.md).
5. Lint + smoke Metro (require del package desde la app).

## Criterios de aceptación

- [ ] Apps importan `@arquetipos/expo-metro-config`.
- [ ] Sin allowlist ESLint para Metro (o documentada si queda tools/).
- [ ] `pnpm nx lint react-native-single|multi` verde.
