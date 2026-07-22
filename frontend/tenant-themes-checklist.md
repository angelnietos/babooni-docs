<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Tenant themes + atmósfera (arquetipos demo)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Contrato de **identidad tenant** (marca) vs **atmósfera tipográfica** (Switch /
Connect / Studio) en apps plantilla multi-tenant. Aplica a Angular, React, Next
e **Ionic** (`ionic-multi` :4301).

## Jerarquía (nunca invertir)

1. **Identidad tenant** (`data-arq-tenant` + tokens de marca) — gana siempre en multi.
2. **Atmósfera** (`data-arq-theme`) — con tenant activo solo cambia **radios /
   densidad**; sin tenant aplica paleta completa Connect/Studio.
3. Defaults `:root` / `variables.css` — fallback.

API shared (`@base/shared`):

| API | Rol |
|-----|-----|
| `setArqDemoTenant` / `applyArqDemoTenant` | `data-arq-tenant` + **inline CSS vars** en `<html>` |
| `setArqVisualTheme` / `applyArqVisualTheme` | `data-arq-theme` + inline; **no borra** marca si hay tenant |

Bootstrap multi (orden fijo):

```ts
applyArqDemoTenant();
applyArqVisualTheme();
```

CSS en `@base/ui-styles`:

| Archivo | Rol |
|---------|-----|
| `themes/tenants.css` | 5 tenants → `--arq-*` / `--color-*` / Ionic primary |
| `themes/arq-atmosphere-overrides.css` | Radios siempre; paleta Connect/Studio solo `:not([data-arq-tenant])` |

**Anti-patrón:** no hardcodear `--arq-brand` en `:host` de páginas Ionic (bloquea
herencia del tenant). Gradientes / FAB / CTAs deben usar
`var(--arq-brand)` + `var(--arq-cta)` / `var(--arq-shadow-glow)`, no teal fijo
(`#14b8a6`).

## Matriz tenants

| Tenant | Brand | CTA | Nav active | Atmósfera visual |
|--------|-------|-----|------------|------------------|
| **nova** | emerald `#047857` | `#059669` | mint soft | esmeralda |
| **helix** | cyan `#0e7490` | coral `#ea580c` | cyan soft | cian + coral |
| **orbit** | copper `#c2410c` | `#ea580c` | peach `#fed7aa` | cobre / champagne |
| **meridian** | ocean `#0369a1` | `#0284c7` | sky soft | océano |
| **lumen** | lime `#4d7c0f` | `#65a30d` | lime soft | lima / crema |

## Atmósfera (Settings)

| Id | Label | Con tenant | Sin tenant |
|----|-------|------------|------------|
| `switch` | Switch | radios generosos | limpia defaults |
| `connect` | Connect | solo radios | cyan frío completo |
| `studio` | Studio | solo radios | ámbar editorial |

Los swatches de la tarjeta Switch pueden seguir siendo teal de preview; el shell
vivo sigue al **tenant**, no al swatch de atmósfera.

## Verify by eye

1. Abrir **angular-multi** / **react-multi** / **ionic-multi** (`:4301`).
2. Login demo: `admin@demo.com` / `admin`.
3. Ajustes → **Identidad tenant** (o switcher del topbar).
4. Confirmar **sin reload**:
   - nav activo (fondo + barra lateral)
   - chip Admin / CTA primary
   - canvas / `--arq-app-canvas`
   - badge «Activo» en la card del tenant
5. Cambiar atmósfera Connect/Studio: radios cambian; **marca tenant no**.
6. Logout → login: hero del tenant.

RN: cards de tenant en `SettingsFeature`; tokens vía `@base/ui-tokens/rn` +
`apps/.../react-native-multi` (nivel token).

## Settings Ionic — paridad web

`@base/ionic-settings-features` monta la misma UX que Angular/React Settings:

- cards de identidad (5 tenants) → `setArqDemoTenant`
- atmósfera Switch / Connect / Studio → `setArqVisualTheme`
- persona demo + matriz RBAC

Styles globales Ionic: `tenants.css` + `arq-atmosphere-overrides.css` en
`project.json` `styles[]` (después de `variables.css`).
