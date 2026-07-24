<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F84-B1 — Migration strategy</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F84" src="https://img.shields.io/badge/round-F84-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · espera F84-A1

## Objetivo

Definir cómo migramos Recalls_v2 a arquetipos minimizando riesgo y downtime.

## Opciones evaluadas

### Big-bang
Reemplazar todo en un corte. Ventajas: simple, sin doble mantenimiento. Desventajas: alto riesgo, requiere feature parity completa antes de ir a prod.

### Strangler fig (recomendada)
Migrar dominio por dominio, arquetipos expone API y frontend路由 por detrás de un API gateway/reverse proxy. El legacy va apagándose progresivamente.

- Fase 1: Auth + base (reemplaza login/users/profiles).
- Fase 2: Campaigns + waves (core).
- Fase 3: Budgets + invoices + reports.
- Fase 4: DGT + addresses.
- Fase 5: Home admin + dashboard.

## Milestones propuestos

| Milestone | Entregas | Gate |
|-----------|----------|------|
| M0 | Setup Nx workspace + adapters base | nx build base-backend verde |
| M1 | Auth domain migrado (login, users, profiles, RBAC) | Tests + coverage verde |
| M2 | Campaigns/Waves migrados (core) | E2E canario verde |
| M3 | Budgets/Invoices migrados | PDF/DOCX generation green |
| M4 | DGT adapter + addresses | SOAP calls mocked → real |
| M5 | Admin/home + reports | Dashboard charts OK |
| M6 | Legacy off | Todo redirect a arquetipos |

## Rollback

Cada milestone deja el legacy funcionando; proxy enruta por feature flag. Rollback = cambiar flag.

## Riesgos

- DGT SOAP: proveedor público, no se puede mockear en prod → parallel run obligatorio.
- MSSQL legacy: F83 habilita MSSQL como adapter; arquetipos puede apuntar a la misma DB durante transición.
- PDFs/DOCX templates: validar paridad visual antes de apagar legacy.

## Criterios

- [ ] Strategy documentada en este plan.
- [ ] Milestones firmados por producto.
- [ ] F84-B1 cerrada después de F84-C1 + F84-D1.
