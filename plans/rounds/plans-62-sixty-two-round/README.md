# Ronda F62 — Features nativas reales + temas + resto F60

## Estado

listo para ejecutar

> Abierta 2026-07-22. Eje: **cerrar la brecha visual** que F60 dejó a medias —
> features feas; temas tenant casi iguales; `/native-ui` piloto F35; `<select>`
> OS; **más el resto F60**: polish átomos native-ui, shell/nav, RN solo tokens,
> Storybook adapters, Chromatic/Code Connect/deprecated, docs.

## Contexto

F60 entregó:

- `@base/ui-styles` 7–1 + composiciones `arq-auth*` / `arq-clients*`
- Login + clients alineados en Angular / React / Next / Ionic
- Tokens teal + e2e SPA verdes

Pero el producto demo **sigue viéndose inconsistente**, y quedan carries F60:

| Problema | Plan F62 |
|----------|----------|
| Temas Helix/Nova casi iguales | A1 |
| Select roles = menú OS | A2 |
| Átomos Lit flat / incompletos (F60-A2) | A3 |
| RN sin espejo tokens nuevos (F60 nota RN) | A4 |
| Users/Roles/Audit/Settings feas | B1 |
| Shell / nav / tenant switcher | B2 |
| `/native-ui` = piloto F35 | C1 |
| Features aún framework UI | C2 |
| Validación isomórfica (F60-E1) | D1 |
| Chromatic / Code Connect / deprecated (F60-E2) | D2 |
| Storybook SoT + adapters brand (F60-B1/B2) | D3 |
| Docs hub (F60-F1) | E1 |

**Contrato F62:** en arquetipos DOM, **toda interacción de producto** usa
`@base/native-ui` (+ wrappers thin). RN = **solo tokens/API**, no Lit.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F62-A1 | [Temas tenant distinguibles + cascade](1750000180000-f62-tenant-themes-cascade.md) | listo para ejecutar |
| F62-A2 | [Native Select listbox](1750000181000-f62-native-select-listbox.md) | listo para ejecutar |
| F62-A3 | [Polish átomos `@base/native-ui`](1750000190000-f62-native-ui-atoms-polish.md) | listo para ejecutar |
| F62-A4 | [RN: paridad tokens (solo tokens)](1750000191000-f62-rn-tokens-parity.md) | listo para ejecutar |
| F62-B1 | [Features users/roles/audit/settings — UI polish](1750000182000-f62-feature-pages-visual-polish.md) | listo para ejecutar |
| F62-B2 | [Shell / nav / tenant switcher](1750000183000-f62-shell-nav-tenant-switcher.md) | listo para ejecutar |
| F62-C1 | [`/native-ui` = catálogo producto real](1750000184000-f62-native-ui-showcase-reality.md) | listo para ejecutar |
| F62-C2 | [Features native-first](1750000185000-f62-features-native-first.md) | listo para ejecutar |
| F62-D1 | [Carry: validación isomórfica](1750000186000-f62-carry-isomorphic-validation.md) | listo para ejecutar |
| F62-D2 | [Chromatic / Code Connect / deprecated](1750000187000-f62-carry-visual-tooling-deprecated.md) | listo para ejecutar |
| F62-D3 | [Storybook SoT + adapters brand](1750000188000-f62-carry-storybook-atoms.md) | listo para ejecutar |
| F62-E1 | [Docs polish + hub F62](1750000189000-f62-documentation-polish.md) | listo para ejecutar |

## Orden sugerido

1. **A1** — temas.
2. **A2 + A3** — select + átomos (paralelo).
3. **A4** — RN tokens (paralelo, no bloquea DOM).
4. **B2** — shell/nav.
5. **C2 → B1** — native-first + polish features.
6. **C1** — showcase `/native-ui`.
7. **D3 → D2** — Storybook luego Chromatic.
8. **D1** — validación (paralelo si hay manos).
9. **E1** — cierre docs.

## Criterios de aceptación (ronda)

- [ ] Temas tenant distinguibles (brand/CTA/nav/focus).
- [ ] Sin `<select>` HTML en features camino feliz; `base-select` listbox.
- [ ] Átomos canario pulidos (A3); Storybook SoT + adapters (D3).
- [ ] Shell/nav/tenant switcher alineados (B2).
- [ ] RN: tokens semánticos espejo documentados (A4); sin Lit.
- [ ] `/native-ui` = catálogo real, no piloto F35.
- [ ] Users/Roles/Audit/Settings look clients + Native*.
- [ ] Chromatic/Code Connect/deprecated: hecho **o** defer F63 explícito (D2).
- [ ] Validación isomórfica: hecho **o** defer F63 (D1).
- [ ] Hubs/biblia → F62; F60 cerrada/carry (E1).

## Fuera de alcance

- Reescribir Josanz/SaaS completo.
- Lit dentro de React Native nativo.
- Rediseño de marketing landing.
- Assets / skins con copyright.
