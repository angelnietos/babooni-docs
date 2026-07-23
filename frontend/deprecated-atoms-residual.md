# Deprecated base atoms — residual consumers (F71-A1)

Inventory of Angular/React atoms marked `@deprecated` (prefer `Native*`
/ `<base-*>`).

## Marked SoT atoms

| Package | Files |
|---------|-------|
| `@base/angular-ui` | `button`, `input`, `alert` components |
| `@base/react-ui` | `Button`, `Input`, `Alert` |

## Residual consumers (2026-07-23)

| Stack | Examples |
|-------|----------|
| Angular base | `domains/auth/.../login-form.component.ts`; `error-banner-presenter`, `confirm-dialog`, `search-bar` |
| React base | `domains/auth/.../LoginForm.tsx`, `roles/.../RoleForm.tsx`, `audit/.../AuditInfo.tsx`; `ErrorState`, `ConfirmDialog`, `SearchBar`, `SessionExpiryHost` |
| Arquetipos features | Mostly **Native** / `ArqNative*` already |
| Josanz | Product `ButtonComponent` in `@josanz/angular-ui` (own branding — not this list) |

## Decision (F71)

| Sub-item | Status | Owner / blocker |
|----------|--------|-----------------|
| Chromatic CI soft | **Defer F72** | Sin `CHROMATIC_PROJECT_TOKEN` / sin paquete chromatic — owner: design-system |
| Code Connect | **Defer F72** | Sin acceso Figma CI — owner: design-system |
| Deprecated migration (auth + chrome) | **Defer F72** | Superficie amplia; arquetipos happy path ya Native-first — owner: FE platform |

F71-A1 cierra sin implementar: cada fila tiene owner y blocker explícitos para F72.
