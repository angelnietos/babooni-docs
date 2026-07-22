<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F68-B3 — Entity-view deepen</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F68" src="https://img.shields.io/badge/round-F68-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Profundizar el rollout de [F67-A3](../plans-67-sixty-seven-round/1750000242000-f67-entity-view-rollout.md):

1. Cablear **users** form view en pages (no solo configs/hooks).
2. Evaluar **audit** (read-only) si aporta EntityViewConfig.
3. Alinear matriz dominio × stack en
   [entity-view-abstractions.md](../../../frontend/entity-view-abstractions.md).

## Entregables

1. Users: form/detail montado desde page/panel (Angular + React mínimo).
2. Audit: decisión explícita (config read-only **o** N/A documentado).
3. Specs de config / helper donde falten.
4. Guía actualizada.

## Criterios de aceptación

- [ ] Users form cableado en ≥1 stack page (no solo hook/config orphan).
- [ ] Audit: hecho **o** N/A con motivo.
- [ ] Matriz dominio × stack al día.

## Verificación

```bash
pnpm nx typecheck base-angular-users-features
pnpm nx typecheck base-react-users-features
```

## Depende de

F67-A3.
