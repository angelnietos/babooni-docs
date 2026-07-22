<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F63 — Overlay hardening + carries visual tooling + polish</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

cerrada (carry → [F64](../plans-64-sixty-four-round/))

> Cerrada 2026-07-22. Entregado parcial: dual shell Ionic, Settings/RBAC mobile,
> cascade tenant > atmósfera (inline CSS + checklist). Verify portal, e2e mobile,
> checkbox/CRUD, carries D1/D2/D3 y brand life residual → **F64**.

## Contexto

F62 cerró el look DOM. F63 abrió overlays + mobile parity + carries; el trabajo
mobile/tenant aterrizó en rama, pero ACs de verify/tooling quedaron abiertos.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F63-A1 | [Select/overlays portal hardening](1750000200000-f63-select-portal-hardening.md) | parcial → [F64-A1](../plans-64-sixty-four-round/1750000210000-f64-select-portal-verify.md) |
| F63-A2 | [Tenant switcher UX](1750000201000-f63-tenant-switcher-ux.md) | parcial (absorbido A1/A3 F64) |
| F63-B1 | [Native checkbox / radio atoms](1750000202000-f63-native-checkbox-radio.md) | trasladado → [F64-B3](../plans-64-sixty-four-round/1750000215000-f64-native-checkbox-crud-density.md) |
| F63-B2 | [CRUD permissions grid + density](1750000203000-f63-crud-permissions-density.md) | trasladado → F64-B3 |
| F63-B3 | [Tenant brand life](1750000204000-f63-tenant-brand-life.md) | parcial (Ionic cascade) → [F64-A3](../plans-64-sixty-four-round/1750000212000-f64-tenant-brand-life-closeout.md) |
| F63-D1 | [Carry: validación isomórfica](1750000205000-f63-carry-isomorphic-validation.md) | defer → [F64-D1](../plans-64-sixty-four-round/1750000218000-f64-carry-isomorphic-validation.md) |
| F63-D2 | [Carry: Chromatic / Code Connect / deprecated](1750000206000-f63-carry-visual-tooling.md) | defer → [F64-C2](../plans-64-sixty-four-round/1750000217000-f64-carry-visual-tooling.md) |
| F63-D3 | [Carry: Storybook SoT + adapters](1750000207000-f63-carry-storybook-atoms.md) | defer → [F64-C1](../plans-64-sixty-four-round/1750000216000-f64-carry-storybook-sot.md) |
| F63-E1 | [Docs polish + hub F63](1750000208000-f63-documentation-polish.md) | hecho (hubs F63; cierre → F64-E1) |
| F63-C1 | [Mobile feature parity + dual shell](1750000209000-f63-mobile-feature-shell-parity.md) | parcial → [F64-B1](../plans-64-sixty-four-round/1750000213000-f64-mobile-e2e-parity.md) |

## Notas de cierre

- Contrato overlays (portal body) y tenant > atmósfera quedan en biblia
  ([tenant-themes-checklist.md](../../../frontend/tenant-themes-checklist.md)).
- Ionic desktop = `lib-app-shell`; móvil = `@base/mobile-chrome` — hecho en C1.
- Carries D* no se dejan “listo eterno”: F64 exige hecho o defer F65 con motivo.

## Fuera de alcance (sigue fuera)

- Rediseño total Josanz/SaaS.
- Sustituir Tailwind en todo el monorepo.
- Marketing / landings.
