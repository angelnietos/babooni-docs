# F58-D1 — MockServer e2e soft + fixtures expand

## Estado

listo para ejecutar

## Objetivo

Ampliar fixtures MockServer (roles/settings CRUD si faltan) y opcional smoke e2e
Playwright contra mock (soft CI o local-only documentado).

Complementa **[F58-D2](1750000159000-f58-arquetipos-e2e-all-apps.md)** (e2e Keycloak
real de todas las apps): D1 = path FE-only; D2 = cobertura de plantillas en CI.

## Entregables

1. Gaps de fixtures vs [auth API contracts] / nav users-audit-roles.
2. Spec soft o script `mockserver:e2e:soft` (login demo → clients).
3. Runbook mockserver actualizado.

## Criterios de aceptación

- [ ] `pnpm mockserver:smoke` sigue verde.
- [ ] Soft e2e o checklist visual ampliada.
- [ ] Sin depender de Keycloak Docker para el path mock.
