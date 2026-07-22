# F60-C2 — Feature nativa Clients list canario

## Estado

listo para ejecutar

## Objetivo

Segunda composición nativa compartida: **lista/empty/error de clients** (flujo
canario de [parity.manifest.yaml](../../../arquetipos/parity.manifest.yaml) y
dual-stack clients), mismo UX en Angular/React; Next/Ionic adoptan; RN espejo
tokens.

## Entregables

1. Composición: page header + toolbar (buscar/crear si aplica) + table/list
   native + empty state + error alert — solo native-ui / wrappers.
2. Contratos de datos vía capas existentes (`*-data-access`); la composición
   es presentacional + slots/callbacks.
3. Paridad comportamiento empty/error documentada
   ([dual-stack-clients-parity](../../../arquetipos/) si existe).
4. Migrar features clients Angular/React arquetipos; thin Next/Ionic.
5. Smoke e2e: post-login `/clients` con `main` visible (suites existentes).

## Criterios de aceptación

- [ ] Angular ↔ React: misma estructura visual de listado canario.
- [ ] Empty y error visibles y consistentes.
- [ ] No HTML/CSS ad-hoc de tabla/botones en features (check ui-ownership /
      native-first).

## Verificación

```bash
pnpm check:arquetipos-parity
pnpm nx run angular-single-e2e:e2e -- --project=chromium
pnpm nx run react-single-e2e:e2e -- --project=chromium
```

## Depende de

F60-A2, idealmente F60-C1 (mismo patrón de composición).

## Bloquea

F60-D1.
