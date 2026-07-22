<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-A3 — Tenant brand life closeout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Cerrar percepción [F63-B3](../plans-63-sixty-three-round/1750000204000-f63-tenant-brand-life.md)
+ regresión del cascade tenant > atmósfera (Ionic/web ya cableados en F63 late).

## Entregables

1. Helix: CTA / Crear primary **coral** evidente (`--arq-cta` / `--color-cta`).
2. Smoke 5 tenants (&lt;2s distinguibles): Nova, Helix, Orbit, Meridian, Lumen —
   Angular + React + Ionic multi.
3. Checklist visual actualizado (capturas o pasos) en
   [tenant-themes-checklist.md](../../../frontend/tenant-themes-checklist.md).
4. Gate blando opcional: script o e2e que lea `--arq-brand` tras `setArqDemoTenant`.

## Criterios de aceptación

- [ ] Helix coral obvio en botón primary.
- [ ] Orbit cobre en nav activo Ionic `:4301` y SPA.
- [ ] Atmósfera Connect/Studio con tenant: solo radios (marca intacta).

## Verificación

Manual switcher + checklist. Opcional:

```bash
pnpm nx serve ionic-multi   # :4301
pnpm nx serve angular-multi # :4203
```

## Depende de

F64-A1 (switcher usable).
