# Ronda F62 — Features nativas reales + temas + resto F60

## Estado

cerrada (carry → [F63](../plans-63-sixty-three-round/))

> Cerrada 2026-07-22. Eje visual DOM entregado: temas tenant, `base-select`,
> shell/nav, features native-first, `/native-ui` catálogo, RN tokens.
> Carries D1/D2/D3 + hardening overlay select → **F63**.

## Contexto

F60 entregó `@base/ui-styles` 7–1 + login/clients. F62 cerró la brecha visual
restante en arquetipos DOM y dejó carries de tooling/validación/Storybook.

**Contrato F62 (cumplido en DOM):** interacción de producto vía `@base/native-ui`
(+ wrappers thin). RN = solo tokens/API, no Lit.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F62-A1 | [Temas tenant distinguibles + cascade](1750000180000-f62-tenant-themes-cascade.md) | hecho |
| F62-A2 | [Native Select listbox](1750000181000-f62-native-select-listbox.md) | hecho (portal body → F63-A1 hardening) |
| F62-A3 | [Polish átomos `@base/native-ui`](1750000190000-f62-native-ui-atoms-polish.md) | hecho (parcial) |
| F62-A4 | [RN: paridad tokens (solo tokens)](1750000191000-f62-rn-tokens-parity.md) | hecho |
| F62-B1 | [Features users/roles/audit/settings — UI polish](1750000182000-f62-feature-pages-visual-polish.md) | hecho |
| F62-B2 | [Shell / nav / tenant switcher](1750000183000-f62-shell-nav-tenant-switcher.md) | hecho |
| F62-C1 | [`/native-ui` = catálogo producto real](1750000184000-f62-native-ui-showcase-reality.md) | hecho |
| F62-C2 | [Features native-first](1750000185000-f62-features-native-first.md) | hecho |
| F62-D1 | [Carry: validación isomórfica](1750000186000-f62-carry-isomorphic-validation.md) | defer → F63-D1 |
| F62-D2 | [Chromatic / Code Connect / deprecated](1750000187000-f62-carry-visual-tooling-deprecated.md) | defer → F63-D2 |
| F62-D3 | [Storybook SoT + adapters brand](1750000188000-f62-carry-storybook-atoms.md) | defer → F63-D3 |
| F62-E1 | [Docs polish + hub F62](1750000189000-f62-documentation-polish.md) | hecho (cierre + handoff F63) |

## Criterios de aceptación (ronda)

- [x] Temas tenant distinguibles (brand/CTA/nav/focus/canvas).
- [x] Sin `<select>` HTML en features camino feliz; `base-select` listbox.
- [x] Shell/nav/tenant switcher BEM + tokens.
- [x] RN: tokens semánticos espejo; sin Lit.
- [x] `/native-ui` = catálogo real.
- [x] Users/Roles/Audit/Settings native-first + `arq-crud*`.
- [x] D1/D2/D3 defer explícito → F63.
- [x] Docs/hubs apuntan a F62 cerrada / F63 activa.

## Notas de cierre

- Bug post-A2: panel `position:fixed` invisible bajo `backdrop-filter` del shell
  → portal a `document.body` (fix en cierre + F63-A1).
- No meter `#` / `color-mix` complejos en arbitrary Tailwind (rompe el CSS bundle).

## Fuera de alcance (sigue fuera)

- Reescribir Josanz/SaaS completo.
- Lit dentro de React Native nativo.
- Rediseño marketing landing.
