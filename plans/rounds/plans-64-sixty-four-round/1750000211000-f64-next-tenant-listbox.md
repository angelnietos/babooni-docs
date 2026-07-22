<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-A2 — Next multi tenant listbox</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Paridad de switcher tenant: SPA usa `base-select` / listbox; `next-multi` aún
usa `<select>` OS en `TenantSwitcherClient.tsx`.

## Entregables

1. Sustituir OS select por listbox nativo (CE `base-select` vía wrapper Next **o**
   composición equivalente sin clip).
2. Aplicar `setArqDemoTenant` / mismos tokens que Angular/React.
3. `next-multi-e2e`: assert cambio de tema (data attr o CSS var) al elegir tenant.
4. Si el runtime Next bloquea CE: excepción documentada en
   [tenant-themes-checklist.md](../../../frontend/tenant-themes-checklist.md) +
   `parity.md` (no dejar silencio).

## Criterios de aceptación

- [ ] Camino feliz Next multi sin `<select>` HTML **o** excepción explícita.
- [ ] Cambiar tenant actualiza marca (nav/CTA/canvas) sin reload.

## Verificación

```bash
pnpm nx serve next-multi
pnpm nx run next-multi-e2e:e2e -- --project=chromium
```

## Depende de

F64-A1 (patrón portal estable).
