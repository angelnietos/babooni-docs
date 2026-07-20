# Nuevo cliente — checklist de producto

**ID plan:** F7-A1 (`F7-A1.cliente-template`)  
**Referencia:** Josanz (`libs/clientes/josanz/`, `apps/clientes/josanz/`)

Guía para añadir un **producto cliente** al monorepo. Los clientes viven en `apps/clientes/{slug}/` + `libs/clientes/{slug}/`, consumen **`@base/*`** y **nunca** importan `@arquetipos/*`.

---

## Convenciones de nombre

| Concepto | Ejemplo | Regla |
|----------|---------|-------|
| **slug** | `acme` | Carpeta en disco, kebab-case, minúsculas |
| **scope npm** | `@acme` | Paquetes `@acme/shared`, `@acme/backend`, `@acme/clients-features` |
| **App Nx backend** | `acme-api` | `apps/clientes/acme/backend` |
| **App Nx frontend** | `acme` | `apps/clientes/acme/frontend/acme` |
| **Tags Nx** | `layer:clientes`, `runtime:*` | Mismo layer que Josanz; ver [check-lib-layout.mjs](../../tools/scripts/check-lib-layout.mjs) |

---

## Árbol mínimo (F7-A1)

```
libs/clientes/{slug}/
├── shared/                 # @acme/shared — DTOs producto (extiende @base/shared)
├── backend/                # @acme/backend — Nest modules producto + re-export @base/backend
├── angular-ui/             # @acme/angular-ui — UI de marca (excepción path raíz, ver F7-S5.ui)
└── angular/
    ├── platform/           # api, data-access, shell — chrome app (nav, auth store, guards)
    └── {domain}/           # api, data-access, shell, features — por dominio de negocio

apps/clientes/{slug}/
├── backend/                # composition root Nest (josanz-api pattern)
└── frontend/{slug}/        # shell Angular delgado (rutas, bootstrap, proxy)
```

Detalle backend: [backend-domain-convention.md](../backend/backend-domain-convention.md).  
Excepciones Josanz aplicables a cualquier cliente: [josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md) (UI raíz, audit thin, users → `@base/users-*`).

---

## Paso 0 — Scaffold automático (opcional)

```bash
# Vista previa
node tools/scripts/scaffold-cliente-product.mjs --slug acme --scope acme --dry-run

# Crear libs mínimas (shared, backend, angular-ui, platform)
node tools/scripts/scaffold-cliente-product.mjs --slug acme --scope acme
```

El script **no** crea apps ni dominios de negocio; genera el esqueleto de libs y lista los pasos manuales restantes.

---

## Paso 1 — Libs base (manual si no usas scaffold)

### 1.1 `@acme/shared`

- Path: `libs/clientes/acme/shared/`
- `package.json`: `"name": "@acme/shared"`, `"@base/shared": "workspace:*"`
- `src/index.ts`: re-export tipos base + tipos exclusivos producto
- Tags: `layer:clientes`, `scope:isomorphic`, `runtime:isomorphic`

### 1.2 `@acme/backend`

- Path: `libs/clientes/acme/backend/src/lib/{domain}/` por dominio producto
- `src/index.ts`: `export * from '@base/backend'` + `JosanzAcme{Domain}Module` pattern → `Acme{Domain}Module`
- App registra módulos en `apps/clientes/acme/backend/src/app/app.module.ts`
- Dominios kernel sin lógica producto: importar `@base/backend` directo (clients, users, billing, …)

### 1.3 `@acme/angular-ui`

- Path: `libs/clientes/acme/angular-ui/` (excepción permanente — no `frontend/angular/ui/`)
- Arranque: re-export `@base/angular-ui` hasta tener Storybook/tokens propios
- App `styles` en `project.json` apunta a `angular-ui/src/lib/styles.scss` cuando exista

### 1.4 `@acme/platform-*`

Copiar estructura de `libs/clientes/josanz/angular/platform/` y renombrar prefijos `josanz` → `acme`:

| Lib | Rol |
|-----|-----|
| `platform-api` | Tipos nav, RBAC, config Figma |
| `platform-data-access` | `GlobalAuthStore`, tenant, plugins |
| `platform-shell` | App shell, sidebar, guards |

---

## Paso 2 — Apps (copiar y adaptar Josanz)

### 2.1 Backend

1. Copiar `apps/clientes/josanz/backend/` → `apps/clientes/acme/backend/`
2. Renombrar proyecto Nx: `josanz-api` → `acme-api` en `project.json`
3. Sustituir imports `@josanz/backend` → `@acme/backend`
4. Ajustar `app.module.ts`: solo módulos que el producto expone
5. `features/`: auth, health, temas — wiring solo de app

### 2.2 Frontend Angular

1. Copiar `apps/clientes/josanz/frontend/josanz/` → `apps/clientes/acme/frontend/acme/`
2. Renombrar proyecto `josanz` → `acme`
3. `app.routes.ts`: lazy `loadChildren` → `@acme/{domain}-shell`
4. Kernel sin libs producto: `@base/users-shell`, `@base/audit-shell` o thin `@acme/audit-*`
5. `tailwind.config.js` / `proxy.conf.json`: paths a libs del nuevo slug
6. (Opcional) e2e: copiar `josanz-e2e` → `acme-e2e`

---

## Paso 3 — Wiring monorepo

### 3.1 `tsconfig.base.json`

Añadir paths (el scaffold imprime bloque listo para pegar):

```json
"@acme/shared": ["./libs/clientes/acme/shared/src/index.ts"],
"@acme/backend": ["./libs/clientes/acme/backend/src/index.ts"],
"@acme/angular-ui": ["./libs/clientes/acme/angular-ui/src/index.ts"],
"@acme/platform-api": ["./libs/clientes/acme/angular/platform/api/src/index.ts"],
"@acme/platform-data-access": ["./libs/clientes/acme/angular/platform/data-access/src/index.ts"],
"@acme/platform-shell": ["./libs/clientes/acme/angular/platform/shell/src/index.ts"]
```

Por cada dominio: `@acme/{domain}-{api,data-access,shell,features}`.

### 3.2 `pnpm install`

Workspace ya incluye `libs/**` y `apps/**`. Tras nuevos `package.json` con `workspace:*`:

```bash
pnpm install
```

### 3.3 ESLint module boundaries

- Tags `layer:clientes` en todas las libs del producto
- El producto **no** puede importar `layer:arquetipos`
- Validar: `node tools/scripts/check-lib-layout.mjs --strict`

---

## Paso 4 — Primer dominio de negocio

```bash
# Adaptar slug en el script o copiar scaffold-josanz-domain y ajustar paths
node tools/scripts/scaffold-josanz-domain.mjs orders "Pedidos"
# Luego mover de josanz/ a acme/ o fork del script → scaffold-cliente-domain.mjs (futuro)
```

Checklist dominio:

1. 4 capas en `libs/clientes/acme/angular/{domain}/`
2. Backend `libs/clientes/acme/backend/src/lib/{domain}/` si hay API propia
3. Paths en `tsconfig.base.json`
4. Ruta en `app.routes.ts` + entrada nav en `platform-shell`
5. `pnpm install` si hay nuevos `workspace:*`

---

## Paso 5 — Verificación

```bash
node tools/scripts/check-lib-layout.mjs --strict
npx tsc -p libs/clientes/acme/shared/tsconfig.lib.json --noEmit
npx tsc -p libs/clientes/acme/backend/tsconfig.lib.json --noEmit
npx tsc -p apps/clientes/acme/frontend/acme/tsconfig.app.json --noEmit
npx tsc -p apps/clientes/acme/backend/tsconfig.app.json --noEmit
pnpm nx serve acme-api
pnpm nx serve acme
```

---

## Qué copiar de plantillas Arquetipos

| Necesidad | Fuente |
|-----------|--------|
| Dominios kernel (auth, clients, users, audit) | `@base/*` + apps `apps/arquetipos/frontend/angular/` |
| Monolith backend wiring | `apps/arquetipos/backend/monolith/api/` |
| Thin domain (solo shell+features) | [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md) |

**No** copiar libs `@arquetipos/*` al producto — usar `@base/*` directo.

---

## Matriz rápida Josanz → nuevo cliente

| Josanz | Nuevo cliente |
|--------|---------------|
| `@josanz/shared` | `@acme/shared` |
| `@josanz/backend` | `@acme/backend` |
| `@josanz/angular-ui` | `@acme/angular-ui` |
| `@josanz/platform-*` | `@acme/platform-*` |
| `@josanz/{domain}-*` | `@acme/{domain}-*` |
| `apps/clientes/josanz/` | `apps/clientes/acme/` |

---

## Enlaces

- [docs/README.md](../README.md) — biblia del monorepo
- [AGENTS.md](../../AGENTS.md)
- [scaffold-cliente-product.mjs](../../tools/scripts/scaffold-cliente-product.mjs)
- [scaffold-josanz-domain.mjs](../../tools/scripts/scaffold-josanz-domain.mjs)
