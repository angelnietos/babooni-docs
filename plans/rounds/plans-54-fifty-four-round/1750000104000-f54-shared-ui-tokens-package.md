# F54-B2 — Paquete `@base/ui-tokens` (web + RN)

## Estado

listo para ejecutar

## Objetivo

F53-E1 deja matriz conceptual. F54 materializa **tokens compartidos**
(colores, radii, spacing, tipografía, semántica tone/size) consumibles por:

- `@base/native-ui` (CSS variables / Lit)
- `@base/react-native-ui` (StyleSheet / theme object)
- Wrappers / marca (opcional re-export)

Evita “cinco paletas” al congelar primitivos framework.

## Diseño propuesto

```
libs/base/frontend/crosscutting/ui-tokens/
  package.json  → @base/ui-tokens
  src/tokens.ts | tokens.css | tokens.rn.ts
```

- **Sin** dependencia de Lit ni React Native en el entry isomorphic.
- RN importa subpath `./rn` si hace falta APIs específicas.

## Tareas

1. Crear lib Nx + exports + paths (`check:exports-paths`).
2. Migrar vars usadas por native-ui al paquete (sin romper temas Josanz).
3. Mapear `@base/react-native-ui` theme a los mismos nombres.
4. Documentar en ui-strategy + add-mobile-domain.
5. Tests: snapshot de keys de tokens (no valores visuales frágiles).

## Criterios de aceptación

- [ ] Paquete `@base/ui-tokens` instalable vía `workspace:*`.
- [ ] native-ui y RN consumen el mismo set de **nombres** canónicos.
- [ ] `pnpm check:exports-paths` verde.
- [ ] Sin Lit en grafo RN.
