<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F64 — Cierre F63 + polish multi-capa</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

> Abierta 2026-07-22. Eje: **cerrar leftovers F63** (overlay verify, mobile e2e,
> carries D1/D2/D3), **endurecer marca tenant** en todos los stacks, y polish
> de calidad (Storybook, CI soft→strict, Next/MF) a cualquier nivel.

## Contexto

F63 entregó (parcial) dual shell Ionic, Settings/RBAC mobile, cascade tenant >
atmósfera (`setArqDemoTenant` inline + CSS). Quedan verificación de overlays,
e2e mobile, carries eternos de validación/Storybook/Chromatic, y drift Next/RN.

| Problema | Plan F64 |
|----------|----------|
| Portal `base-select` implementado; ACs manuales + stories abiertos (F63-A1) | A1 |
| Next multi sigue con `<select>` OS; SPA ya usa listbox | A2 |
| Helix coral / contraste 5 tenants “vida” (F63-B3 residual) | A3 |
| Ionic `/clients` e2e + routes en parity manifest (F63-C1) | B1 |
| RN/Ionic leftovers indigo + avatar + specs typecheck | B2 |
| Roles/settings: checkbox nativo + densidades CRUD (F63-B1/B2) | B3 |
| Storybook SoT + orphans (F63-D3 / F62-D3) | C1 |
| Chromatic / Code Connect / `@deprecated` (F63-D2) | C2 |
| Validación isomórfica (F59→F63 defer) | D1 |
| Jest coverage / CI soft→strict ratchet | D2 |
| Docs + hub F64 / cierre F63 | E1 |

**Contrato (hereda F62/F63):**

1. Overlays de producto → portal a `body` (no clip por `backdrop-filter`).
2. Identidad tenant **gana** sobre atmósfera tipográfica
   ([tenant-themes-checklist.md](../../../frontend/tenant-themes-checklist.md)).
3. Carries D1/C1/C2: **hecho o defer F65 con motivo** — no “listo eterno”.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F64-A1 | [Select portal verify + regression](1750000210000-f64-select-portal-verify.md) | listo |
| F64-A2 | [Next multi tenant listbox](1750000211000-f64-next-tenant-listbox.md) | listo |
| F64-A3 | [Tenant brand life closeout](1750000212000-f64-tenant-brand-life-closeout.md) | listo |
| F64-B1 | [Mobile e2e + parity manifest](1750000213000-f64-mobile-e2e-parity.md) | listo |
| F64-B2 | [RN/Ionic token + typecheck hygiene](1750000214000-f64-mobile-token-typecheck.md) | listo |
| F64-B3 | [Native checkbox/radio + CRUD density](1750000215000-f64-native-checkbox-crud-density.md) | listo |
| F64-C1 | [Carry: Storybook SoT + adapters](1750000216000-f64-carry-storybook-sot.md) | listo |
| F64-C2 | [Carry: Chromatic / Code Connect / deprecated](1750000217000-f64-carry-visual-tooling.md) | listo |
| F64-D1 | [Carry: validación isomórfica](1750000218000-f64-carry-isomorphic-validation.md) | listo |
| F64-D2 | [CI soft→strict ratchet](1750000219000-f64-ci-soft-strict-ratchet.md) | listo |
| F64-E1 | [Docs polish + hub F64](1750000220000-f64-documentation-polish.md) | listo |

## Orden sugerido

1. **A1** — verify portal (bloquea percepción switcher + Storybook select).
2. **A2 / A3** — Next + brand life (paralelo a B*).
3. **B1 → B2 → B3** — mobile e2e, tokens, checkbox/CRUD.
4. **C1 → C2** — Storybook luego Chromatic/deprecated.
5. **D1 / D2** — validación + CI ratchet (paralelo).
6. **E1** — cierre docs.

## Criterios de aceptación (ronda)

- [ ] Tenant switcher SPA: listbox visible; 5 tenants pintan marca sin reload.
- [ ] `next-multi` sin `<select>` OS en camino feliz **o** excepción documentada.
- [ ] Ionic soft e2e: login → `/clients`; parity manifest incluye rutas mobile.
- [ ] `rg "#4f46e5|#6366f1" libs/base/frontend/mobile` limpio (o tokenizado).
- [ ] C1/C2/D1: hecho **o** defer F65 con motivo en hub.
- [ ] Hubs → F64 activa; F63 cerrada.

## Fuera de alcance

- Rediseño total Josanz/SaaS.
- Lit dentro de React Native nativo.
- Marketing / landings.
- Sustituir Tailwind en todo el monorepo.
