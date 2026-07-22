<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F44-A1 — Inventario de deprecations activas</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Resumen

| Métrica | Valor |
|---------|-------|
| Entradas inventariadas | 9 |
| `keep` | 9 |
| `remove-next-minor` | 0 |
| `remove-next-major` | 0 |
| JSDoc con fecha explícita | 9 |
| JSDoc con reemplazo | 9 |
| JSDoc con motivo | 9 |

## Tabla de deprecations

| # | Archivo:linea | Símbolo / API | Fecha deprecación | Reemplazo | Motivo | Estado | Notas JSDoc |
|---|---------------|---------------|-------------------|-----------|--------|--------|-------------|
| 1 | `libs/base/backend/src/lib/domains/clients/application/clients.dtos.ts:111` | `CreateClientCommand` | 2026-07-17 | `CreateClientDto` | Alias de clase mantenido por compatibilidad con call sites gRPC / OpenAPI contract tests | `keep` | JSDoc completo: sí. |
| 2 | `libs/base/backend/src/lib/domains/clients/application/clients.dtos.ts:116` | `UpdateClientCommand` | 2026-07-17 | `UpdateClientDto` | Alias de clase mantenido por compatibilidad | `keep` | JSDoc completo: sí. |
| 3 | `libs/base/backend/src/lib/domains/clients/application/clients.dtos.ts:119` | `FindClientByEmailCommand` | 2026-07-17 | `FindClientByEmailDto` | Alias de clase mantenido por compatibilidad | `keep` | JSDoc completo: sí. |
| 4 | `libs/base/backend/src/lib/domains/clients/application/clients.dtos.ts:122` | `DeleteClientCommand` | 2026-07-17 | `DeleteClientDto` | Alias de clase mantenido por compatibilidad | `keep` | JSDoc completo: sí. |
| 5 | `libs/base/frontend/angular/platform/ui/src/lib/badge/badge.component.ts:7` | `BadgeTone` (Angular) | 2026-07-17 | `BadgeColor` | Alias de tipo mantenido para compatibilidad | `keep` | JSDoc completo: sí. |
| 6 | `libs/base/frontend/react/platform/ui/src/lib/badge/Badge.tsx:12` | `BadgeProps.tone` (React) | 2026-07-17 | `color` | Prop mantenida como backwards-compatible | `keep` | JSDoc completo: sí. |
| 7 | `libs/base/frontend/react/platform/ui/src/lib/badge/Badge.tsx:17` | `BadgeTone` (React) | 2026-07-17 | `BadgeColor` | Alias de tipo mantenido para compatibilidad | `keep` | JSDoc completo: sí. |
| 8 | `libs/clientes/josanz/angular-ui/src/lib/catalog/status-pill-presets.ts:100` | `leadingMarkGradientStyle` | 2026-07-10 | `leadingMarkAvatarStyle` | Función alias mantenida; el reemplazo usa solo color rail sin gradiente | `keep` | JSDoc completo: sí. |
| 9 | `libs/clientes/josanz/angular-ui/src/lib/overlays/theme-modal/theme-modal.ts:6` | `ThemeModalComponent` | 2026-07-10 | Ajustes → pestaña Personalización | Componente mantenido por compatibilidad en Storybook | `keep` | JSDoc completo: sí. |

## Hallazgos de JSDoc

Criterio: todo `@deprecated` debe incluir en su JSDoc **fecha**, **reemplazo** y **motivo**.

- **Fecha de deprecación**: 9/9 entradas incluyen fecha en el comentario JSDoc.
- **Reemplazo**: 9/9 entradas especifican el símbolo o API de reemplazo.
- **Motivo**: 9/9 entradas incluyen un motivo explícito.

## Acciones recomendadas

1. Revisar call sites activos de las entradas marcadas como `keep` para confirmar que el reemplazo está adoptado en el próximo ciclo.
2. Monitorear usos en productos SaaS y arquetipos antes de promover a `remove-next-minor`.
