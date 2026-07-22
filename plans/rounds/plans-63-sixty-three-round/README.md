# Ronda F63 — Overlay hardening + carries visual tooling + polish

## Estado

listo para ejecutar

> Abierta 2026-07-22. Eje: **overlays nativos fiables** (select/tenant), carries
> F62 (validación isomórfica, Chromatic/Code Connect, Storybook), y polish UI
> que F62 dejó a medias (checkbox nativo, grid permisos, CTA Helix coral).

## Contexto

F62 cerró el look DOM (temas, shell, features native-first, `/native-ui`). Quedan:

| Problema | Plan F63 |
|----------|----------|
| `base-select` panel invisible bajo `backdrop-filter` / overflow shell | A1 |
| Tenant switcher UX (label clip, focus, teclado) | A2 |
| Checkboxes/radios nativos vs inputs OS en roles/settings | B1 |
| Grid permisos / densidades CRUD | B2 |
| Helix coral CTA + contraste tenant “vida” | B3 |
| Validación isomórfica (F62-D1) | D1 |
| Chromatic / Code Connect / deprecated (F62-D2) | D2 |
| Storybook SoT + adapters (F62-D3) | D3 |
| Feature parity mobile (shell dual + TEMPLATE_NAV) | C1 |

**Contrato:** overlays de producto (`base-select`, futuros menus/popovers) **siempre**
escapan el containing block del shell (portal a `body` o estrategia equivalente).

Ionic/RN deben exponer las **mismas features** que el SPA (`TEMPLATE_NAV`); Ionic
desktop = `lib-app-shell`; móvil = `@base/mobile-chrome` / `ArqMobileShell`.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F63-A1 | [Select/overlays portal hardening](1750000200000-f63-select-portal-hardening.md) | en progreso |
| F63-A2 | [Tenant switcher UX](1750000201000-f63-tenant-switcher-ux.md) | listo |
| F63-B1 | [Native checkbox / radio atoms](1750000202000-f63-native-checkbox-radio.md) | listo |
| F63-B2 | [CRUD permissions grid + density](1750000203000-f63-crud-permissions-density.md) | listo |
| F63-B3 | [Tenant brand life (Helix coral + contrast)](1750000204000-f63-tenant-brand-life.md) | listo |
| F63-D1 | [Carry: validación isomórfica](1750000205000-f63-carry-isomorphic-validation.md) | listo |
| F63-D2 | [Carry: Chromatic / Code Connect / deprecated](1750000206000-f63-carry-visual-tooling.md) | listo |
| F63-D3 | [Carry: Storybook SoT + adapters](1750000207000-f63-carry-storybook-atoms.md) | listo |
| F63-E1 | [Docs polish + hub F63](1750000208000-f63-documentation-polish.md) | listo |
| F63-C1 | [Mobile feature parity + dual shell](1750000209000-f63-mobile-feature-shell-parity.md) | en progreso |

## Orden sugerido

1. **A1** — select portal (bloquea percepción tenant switcher).
2. **A2** — UX switcher.
3. **B1 → B2 → B3** — polish features / marca.
4. **D3 → D2** — Storybook luego Chromatic.
5. **D1** — validación (paralelo).
6. **E1** — cierre docs.

## Criterios de aceptación (ronda)

- [ ] Tenant switcher abre listbox visible con todas las orgs; cambia tema al elegir.
- [ ] Ningún overlay de native-ui queda clipado por shell `backdrop-filter`.
- [ ] Roles/settings usan checkbox/radio nativos (no OS planos) en camino feliz.
- [ ] Helix muestra coral en CTA de forma obvia.
- [ ] D1/D2/D3: hecho **o** defer F64 con motivo.
- [ ] Hubs → F63 activa; F62 cerrada.

## Fuera de alcance

- Rediseño total Josanz/SaaS.
- Sustituir Tailwind en todo el monorepo.
- Marketing / landings.
