<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F79-B2 — Ionic Lit adapters (`@base/ionic-ui`)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F79" src="https://img.shields.io/badge/round-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24 · Depende de: [F79-A1](1761000001000-f79-atom-matrix-and-migration-contract.md)

## Objetivo

Separar **átomos de diseño** (Lit SoT) de **chrome Ionic** (`ion-header`, `ion-tabs`, gestos, etc.) en `@base/ionic-ui`, para paridad visual con Angular/React/Next.

## Paths

- Lib: `libs/base/frontend/mobile/ionic/ui/` (`base-ionic-ui`, `@base/ionic-ui`)
- Referencia Angular wrappers: `libs/base/frontend/angular/platform/ui/`
- Apps: arquetipos Ionic single/multi

## Problema

`ArqIonButton`, `ArqIonInput`, `ArqIonBadge`, … delegan en `ion-*`, que **no** es el SoT Lit. Resultado: look Ionic distinto al resto de Arquetipos.

## Estrategia

1. **Átomos SoT** (`button`, `input`, `badge`, `alert` inline, `card`, …):
   - Preferir CE Lit en template Angular (`CUSTOM_ELEMENTS_SCHEMA` / wrappers existentes de `@base/angular-ui`).
   - Mantener nombre público `ArqIon*` o introducir `ArqNative*` + alias deprecation — documentar en matriz.
2. **Chrome / mobile UX** (`ArqIonTabs`, `ArqIonFeatureShell`, `ArqIonPageHeader`, searchbar keyboard-aware):
   - Pueden seguir en `ion-*`; no forzar Lit.
3. **Overlays**:
   - `ion-alert` / `ion-modal` vs Lit `base-alert` / `base-modal`: elegir por UX móvil (a11y, focus trap). Si se mantiene Ion overlay, documentar como **chrome**, no como SoT atom; Storybook lo etiqueta distinto.
4. Tema: mapear CSS vars Ionic → `--arq-*` / tokens existentes (`theme.ts`).

### Oleadas

| Wave | Scope |
|------|-------|
| W1 | Button, Input, Badge, Card |
| W2 | EmptyState, Skeleton, Avatar, Segment |
| W3 | Alert/Modal policy (Lit vs Ion overlay) + Toast/feedback |
| W4 | Rest SoT atoms + deprecate ion-as-atom |

## Acciones

- [ ] Clasificar cada `ArqIon*` en matriz (SoT vs chrome)
- [ ] Migrar W1–W4; actualizar templates Ionic features que hardcodeen `ion-button` para átomos
- [ ] Ajustar stories Ionic (F79-C1)
- [ ] Smoke: `ionic-single` / e2e smoke si existe

## Criterios de aceptación

- [ ] Átomos SoT Ionic renderizan Lit (o wrapper Angular CE) en demos/Storybook
- [ ] Chrome Ion documentado y no contado como “gap de paridad”
- [ ] `pnpm nx typecheck base-ionic-ui` verde
- [ ] Apps Ionic arquetipos arrancan sin regresión de navegación

## Verificación

```bash
pnpm nx typecheck base-ionic-ui
pnpm storybook:base-ionic
# smoke serve ionic canario
```

## Riesgos

- Mezclar Lit + Ion shadow DOM → verificar CSS vars / `::part` si aplica
- Regresión a11y en overlays → tests manuales W3
