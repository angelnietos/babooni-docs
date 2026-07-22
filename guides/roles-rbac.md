<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Roles & RBAC (Arquetipos)</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Model

| Layer | Role |
|-------|------|
| **Keycloak** | Identity only (login, JWT `realm_access.roles`) |
| **Prisma `Role`** | Source of truth for app permissions + fieldPolicies |
| **`ROLE_PERMISSIONS`** | Defaults for system roles (`admin` / `manager` / `member`) and fallback when DB is empty |

### Role row

- `name` — `admin` \| `manager` \| `member` (or custom)
- `label` — display name
- `system` — protected from rename/delete
- `permissions` — `String[]` from `PERMISSIONS` catalog
- `fieldPolicies` — ABAC-lite JSON: `{ resource, field, access: view|mask|edit|hide }`

## Runtime flow

1. JWT validated (`JwtStrategy`) → `request.user`
2. `PermissionsGuard` enriches user from DB (`user.email` → `User.role` → permissions/fieldPolicies)
3. `@RequirePermission(...)` checks effective permissions
4. `PiiMaskInterceptor` masks/hides response PII per fieldPolicies
5. SPA calls `GET /api/auth/me` to sync PermissionService / React `PermissionProvider`

## Demo personas (seed)

| User | Role | Expect |
|------|------|--------|
| `admin@demo.com` | admin | Full nav incl. Roles; write everywhere |
| `demo@demo.com` | manager | Clients write, Users/Audit read; no Roles write |
| `readonly@demo.com` | member | Clients/Settings read; emails masked; no Users/Audit/Roles; no write buttons |

Seed: `tools/seeds/seed-arquetipos-demo.sql` (run after migrate).

```bash
pnpm migrate:deploy
pnpm migrate:deploy:single
# then apply seed SQL against your DB
```

## Adding a permission

1. Add key to `PERMISSIONS` in `libs/base/shared/src/lib/rbac/permissions.ts`
2. Update `ROLE_PERMISSIONS` defaults for system roles
3. Decorate controller methods with `@RequirePermission(PERMISSIONS.X)`
4. Gate FE nav (`TEMPLATE_NAV_ITEMS`) and write actions with `can()` / `usePermission`
5. Re-seed or update Role.permissions for existing system roles

## FE packages

- `@base/roles-{api,data-access,shell,features}` + `@base/react-roles-*`
- Thin: `@arquetipos/angular-roles-*` / `@arquetipos/react-roles-*`
- Routes wired in angular-single/multi and react-single/multi

## Smoke checklist

- [ ] Admin: sidebar shows Roles; create custom role; assign via Users (roleId)
- [ ] Manager: Clients Crear/Eliminar visible; Roles nav hidden (no roles:read)
- [ ] Member: no Crear/Eliminar on Clients; emails look like `a***@demo.com`; Users/Audit/Roles routes denied
- [ ] `GET /api/auth/me` returns permissions + fieldPolicies matching DB role
