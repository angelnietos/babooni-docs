# Deprecated base atoms — residual consumers (F69-B1)

Inventory of Angular/React atoms marked `@deprecated` (prefer `Native*` /
`<base-*>`).

## Marked SoT atoms

| Package | Files |
|---------|-------|
| `@base/angular-ui` | `button`, `input`, `alert` components |
| `@base/react-ui` | `Button`, `Input`, `Alert` |

## Residual consumers (2026-07-22)

| Stack | Examples |
|-------|----------|
| Angular base | `domains/auth/.../login-form.component.ts`; `error-banner-presenter`, `confirm-dialog`, `search-bar` |
| React base | `domains/auth/.../LoginForm.tsx`, `roles/.../RoleForm.tsx`, `audit/.../AuditInfo.tsx`; `ErrorState`, `ConfirmDialog`, `SearchBar`, `SessionExpiryHost` |
| Arquetipos features | Mostly **Native** / `ArqNative*` already |
| Josanz | Product `ButtonComponent` in `@josanz/angular-ui` (own branding — not this list) |

## Decision (F68 close → F69 → F70)

| Sub-item | Status | Owner / blocker |
|----------|--------|-----------------|
| Chromatic CI soft | **Defer F70** | Sin `CHROMATIC_PROJECT_TOKEN` / sin paquete chromatic — owner: design-system |
| Code Connect | **Defer F70** | Sin acceso Figma CI — owner: design-system |
| Deprecated migration (auth + chrome) | **Defer F70** | Superficie amplia; arquetipos happy path ya Native-first — owner: FE platform |

F69-B1 no pudo cerrar por blockers externos. Reanudar en F70 con owner asignado.
