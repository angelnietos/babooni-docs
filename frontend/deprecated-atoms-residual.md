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

## Decision (F68 close → F69)

| Sub-item | Status | Owner / blocker |
|----------|--------|-----------------|
| Chromatic CI soft | **Target F69-B1** | No `CHROMATIC_PROJECT_TOKEN` / no chromatic package — owner: design-system |
| Code Connect | **Target F69-B1** | No Figma CI access — owner: design-system |
| Deprecated migration (auth + chrome) | **Target F69-B1** | Wide surface; arquetipos happy path already Native-first — owner: FE platform |

See also [design-system.md](./design-system.md) · [ci-gates.md](./ci-gates.md) ·
[F69-B1](../plans/rounds/plans-69-sixty-nine-round/1750000263000-f69-carry-chromatic-deprecated.md).
