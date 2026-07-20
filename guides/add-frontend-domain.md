# Añadir un dominio frontend

Receta para un **dominio de producto** (patrón Josanz). Para plantillas Arquetipos thin, ver [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md).

---

## Antes de empezar

1. Elige un **slug** único (`orders`, `fleet`, …) — plural en rutas HTTP (`/api/orders`).
2. Confirma si el dominio es **solo UI** (consume API existente) o necesita **backend nuevo** → [add-backend-domain.md](./add-backend-domain.md).
3. UI de marca → `@josanz/angular-ui`. Genérico → `@base/angular-ui`.

---

## Estructura objetivo (Angular producto)

```
libs/clientes/josanz/angular/{domain}/
├── api/src/              @josanz/{domain}-api
├── data-access/src/      @josanz/{domain}-data-access
├── shell/src/            @josanz/{domain}-shell  → lazy features
└── features/src/
    ├── layout/
    ├── pages/
    ├── components/
    ├── {domain}.routes.ts
    └── index.ts
```

**Por qué cada capa:** ver [architecture/overview.md § Frontend](../architecture/overview.md#4-frontend--cuatro-capas-por-dominio).

---

## Pasos

### 1. Scaffold (recomendado)

```bash
node tools/scripts/scaffold-josanz-domain.mjs orders "Pedidos"
```

Revisa paths generados; ajusta nombres de rutas y DTOs.

### 2. Manual — checklist

1. **api** — tipos en `@josanz/shared` o re-export `@base/shared`.
2. **data-access** — servicio HTTP + store (NgRx). Usa `@base/angular-api` `ApiClient`.
3. **features** — `layout/` (chrome), `pages/` (rutas), `components/` (smart, solo compone UI).
4. **shell** — `{domain}ShellRoutes` con `loadChildren` → `@josanz/{domain}-features`.
5. **tsconfig.base.json** — paths `@josanz/{domain}-*`.
6. **package.json** en cada lib — `workspace:*` deps.
7. **App** — `apps/clientes/josanz/frontend/josanz/src/app/app.routes.ts`:

```typescript
{
  path: 'orders',
  loadChildren: () =>
    import('@josanz/orders-shell').then((m) => m.ordersShellRoutes),
},
```

8. **Nav** — entrada en `platform-shell` si aplica RBAC/menú.

### 3. UI ownership

Consulta [ui-component-catalog.yaml](../frontend/ui-component-catalog.yaml). Valida:

```bash
node tools/scripts/check-ui-ownership.mjs
node tools/scripts/check-frontend-conventions.mjs
```

**Regla:** features `components/` importan `@josanz/angular-ui` (producto) o `@base/angular-ui` (kernel). No mezclar capas arquetipos ↔ clientes.

---

## React (base / arquetipos)

Misma forma lógica; paquetes `@base/{domain}-{data-access,shell,features}` o thin `@arquetipos/react-{domain}-shell`.

```bash
npx tsc -p libs/base/frontend/react/clients/features/tsconfig.lib.json --noEmit
```

Apps plantilla: lazy `@arquetipos/react-{domain}-shell`.

---

## Excepciones Josanz

| Dominio | Tratamiento |
|---------|-------------|
| `audit`, `users` | Thin 4 capas → `@base/*` — [josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md) |
| `platform/` | Sin `features/` — chrome global |

---

## Verificación

```bash
npx tsc -p libs/clientes/josanz/angular/{domain}/features/tsconfig.lib.json --noEmit
npx tsc -p apps/clientes/josanz/frontend/josanz/tsconfig.app.json --noEmit
node tools/scripts/check-frontend-conventions.mjs
```

---

## Enlaces

- [add-backend-domain.md](./add-backend-domain.md)
- [backend-domain-convention.md](../backend/backend-domain-convention.md)
