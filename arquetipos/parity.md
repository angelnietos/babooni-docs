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
pnpm check:arquetipos-parity          # soft (CI)
pnpm check:arquetipos-parity -- --strict
```

## Niveles por stack

| Nivel | Stacks | Exigencia |
|-------|--------|-----------|
| Full | Angular / React SPA | 4 capas + UI native + rutas |
| Core | Next | Subset |
| Mobile | Ionic | CE / native en hosts DOM |
| Token | RN | API/tokens alineados (`@base/ui-tokens`); **no** Lit |

## Visual smoke

Con native-ui compartido, el drift de átomos es bajo. Playwright multi-stack
queda **defer** (F55) salvo regresión de layout de página.
