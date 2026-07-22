<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F65-D1 — Front state + SoC multi-stack (swappable / testable)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F65" src="https://img.shields.io/badge/round-F65-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Pulir la **gestión de estado** y la **separación de responsabilidades** en el
front de **todas** las apps arquetipos (Angular, React, Ionic, Next, RN) para:

1. **Cambiar la herramienta** de estado/datos sin reescribir features.
2. **Testear** paneles con mocks de puertos.

## Entregables

### D1.1 — Contrato de dominio (piloto `clients`)

- Angular: `ClientsFacade` (= `ClientsService` / EntityListStore); panel sin NgRx.
- React: `useClientsFacade` (react-query); panel sin `useAppDispatch`.
- Ionic: `IonicClientsFacade` (signals).
- Next / RN: `useClientsFacade` (list state in data-access).

### D1.2 — Guía

- [state-soc-facade.md](../../../frontend/state-soc-facade.md)
- Guías `add-frontend-domain` / `add-mobile-domain` actualizadas.

### D1.3 — Tests

- React / Next / RN / Ionic features mockean el facade (sin RTK/NgRx TestBed completo).

## Criterios de aceptación

- [x] Features `clients` (5 stacks) consumen facade/puerto.
- [x] Herramienta canónica documentada; dualidad NgRx/RTK list `@deprecated` en piloto.
- [x] Tests con mock del puerto (≥2 stacks).
- [x] Guía publicada; hub F65 cerrado.
- [x] Check ESLint features↔store → F66 (opcional ratchet).

## Verificación

```bash
pnpm nx typecheck arquetipos-angular-clients-features
pnpm nx typecheck arquetipos-react-clients-features
pnpm nx typecheck base-ionic-clients-features
pnpm nx typecheck base-next-clients-features
pnpm nx typecheck base-react-native-clients-features
pnpm nx test base-react-clients-features
pnpm nx test base-ionic-clients-features
pnpm nx test base-next-clients-features
```

## Depende de

F65-A* (confirm/toast). ADR 0006 + F41.

## Fuera de alcance

- Migrar Josanz/SaaS al mismo facade en esta tarjeta.
- Check tools/checks warn→strict (F66).
