<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F67-B3 — Features ↔ store ratchet expand</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F67" src="https://img.shields.io/badge/round-F67-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Ampliar el ratchet de F66-B3 (`tools/checks/check-features-no-store.mjs`):
con facade canónico en **users / roles / audit**, las `*-features` de esos
dominios no deben importar stores concretos de list CRUD.

## Resultado (2026-07-22)

`STRICT_DOMAINS` ya incluye clients / users / roles / audit; check verde.
Guía [state-soc-facade.md](../../../frontend/state-soc-facade.md) actualizada.

## Entregables

1. Inventario: imports de `Store` / `useAppDispatch` / `*Store` en features
   users/roles/audit (Angular + React; Ionic si aplica).
2. Refactor a facade donde falte (pages/hooks ya deberían usarlo tras F66-D1).
3. Ampliar strict allowlist → fail en CI soft/job frontend para esos dominios.
4. Actualizar [state-soc-facade.md](../../../frontend/state-soc-facade.md).

## Criterios de aceptación

- [x] `pnpm check:features-no-store` (o `:strict`) verde para clients **y**
      users/roles/audit (Angular + React) **o** excepciones documentadas +
      defer F68.
- [x] Allowlist temporal acotada y con fecha/owner.
- [x] Script enlazado desde hygiene / docs CI.

## Verificación

```bash
pnpm check:features-no-store
pnpm check:features-no-store:strict
```

## Depende de

F66-B3 + F66-D1.
