# Deprecated base atoms — residual consumers (F66-B1)

Inventory of Angular/React atoms marked `@deprecated` (prefer `Native*` /
`<base-*>`). Carry from F65-B2 — target cleanup **F66-B1** (or defer F67).

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

## Decision

| Sub-item | Status | Owner / blocker |
|----------|--------|-----------------|
| Chromatic CI soft | **F66-B1** | No `CHROMATIC_PROJECT_TOKEN` / no chromatic package |
| Code Connect | **F66-B1** | No Figma CI access |
| Deprecated migration (auth + chrome) | **F66-B1** | Wide surface; arquetipos happy path already Native-first |

See also [design-system.md](./design-system.md) · [ci-gates.md](./ci-gates.md) ·
[F66-B1](../plans/rounds/plans-66-sixty-six-round/1750000232000-f66-carry-chromatic-deprecated.md).
