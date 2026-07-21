# Paridad arquetipos (F54-B4)

Las plantillas `apps/arquetipos/**` + libs `@arquetipos/*` deben ser **lo más
parecidas posible** en tres ejes:

| Eje | Contrato | Tooling |
|-----|----------|---------|
| **Arquitectura** | 4 capas por dominio (`api` → `data-access` → `features` ← `ui`), shells lazy, apps thin, thin → `@base/*` | `check:arquetipos-parity` + `check:lib-layout` |
| **Visual** | SoT `@base/native-ui` vía `Native*` / `ArqNative*` / CE (ADR 0010) | `check:ui-native-first` |
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
| Mobile | Ionic | CE / native en hosts DOM |
| Token | RN | API/tokens alineados (`@base/ui-tokens`); **no** Lit |

## Visual / layout smoke (F55-B2)

Secuencia canario **idéntica** en `angular-single-e2e` y `react-single-e2e`
(`src/smoke.spec.ts`):

1. Unauthenticated `/clients` → redirect `/auth/login`
2. Login demo → `/clients`, `/users`, `/audit` con `main` visible
3. Login readonly → sin link Audit; Clients visible

CI `e2e-web` (main only) ejecuta angular-single + multi stacks. Screenshots
opcionales / Chromatic → [F55-B1](../plans/rounds/plans-55-fifty-five-round/1750000122000-f55-chromatic-visual-regression.md) (defer token).
