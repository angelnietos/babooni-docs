# F51-C1 — Paquete `@arquetipos/expo-metro-config` (opcional)

## Estado

completado

## Objetivo

Hoy el helper vive en `tools/metro/…` con allowlist ESLint. Preferible un
paquete workspace `@arquetipos/expo-metro-config` importado por nombre
(sin relative fuera del project root ni allow especial).

## Resultado

- Paquete: `libs/arquetipos/frontend/mobile/rn/metro-config` →
  `@arquetipos/expo-metro-config` (`index.cjs`).
- Apps `react-native-single|multi`: dep `workspace:*` +
  `require('@arquetipos/expo-metro-config')`.
- Allowlist ESLint de `tools/metro/…` eliminada.
- `tools/metro/create-arquetipos-expo-metro-config.cjs` → re-export deprecado
  (relative al package).
- Guía [add-mobile-domain.md](../../../guides/add-mobile-domain.md) actualizada.
- Lint verde en ambas apps RN.

## Criterios de aceptación

- [x] Apps importan `@arquetipos/expo-metro-config`.
- [x] Sin allowlist ESLint para Metro.
- [x] `pnpm nx lint react-native-single|multi` verde.
