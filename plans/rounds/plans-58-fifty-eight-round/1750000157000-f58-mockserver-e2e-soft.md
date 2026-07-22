# F58-D1 — MockServer e2e soft + fixtures expand

## Estado

listo para ejecutar

## Objetivo

Ampliar fixtures MockServer (roles/settings CRUD si faltan) y opcional smoke e2e
Playwright contra mock (soft CI o local-only documentado).

## Entregables

1. Gaps de fixtures vs [auth API contracts] / nav users-audit-roles.
2. Spec soft o script `mockserver:e2e:soft` (login demo → clients).
3. Runbook mockserver actualizado.

## Criterios de aceptación

- [ ] `pnpm mockserver:smoke` sigue verde.
- [ ] Soft e2e o checklist visual ampliada.
- [ ] Sin depender de Keycloak Docker para el path mock.
