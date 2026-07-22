# Scaffold Arquetipos (dominio + app)

Utilidades para generar plantillas thin sobre `@base/*` y clonar apps de referencia.

---

## Dominio frontend (4 capas)

```bash
node tools/scaffolds/scaffold-arquetipos-domain.mjs <domain> \
  [--framework angular|react|both] \
  [--with-backend] \
  [--title "Display Title"] \
  [--dry-run]
```

Ejemplo:

```bash
node tools/scaffolds/scaffold-arquetipos-domain.mjs orders --framework both --dry-run
node tools/scaffolds/scaffold-arquetipos-domain.mjs orders --with-backend
```

Crea `libs/arquetipos/frontend/{angular|react}/{domain}/{api,data-access,shell,features}` con re-exports thin hacia `@base/*`. Con `--with-backend` invoca `new-domain.mjs`.

Después:

1. `pnpm install`
2. `node tools/migrate/emit-frontend-tsconfig-paths.mjs` (si aplica)
3. Cablear la app a `@arquetipos/{angular|react}-{domain}-shell`

Detalle del modelo thin (incl. **override de un método** con flag
`ARQ_CLIENTS_DATA_ACCESS`): [arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md).

---

## App desde plantilla

```bash
node tools/scaffolds/scaffold-arquetipos-app.mjs <kebab-name> \
  [--framework angular|react] \
  [--tenant single|multi] \
  [--dry-run]
```

Ejemplo:

```bash
node tools/scaffolds/scaffold-arquetipos-app.mjs demo --framework angular --tenant single --dry-run
```

Clona `angular-single` / `angular-multi` / `react-single` / `react-multi` bajo
`apps/arquetipos/frontend/{framework}/{tenant}-tenant/{name}/`, renombrando proyecto y rutas embebidas.

Ajusta el `serve.port` en `project.json` si choca con la plantilla origen.

---

## Relacionados

| Script | Uso |
|--------|-----|
| `scaffold-josanz-domain.mjs` | Dominio producto Josanz |
| `scaffold-cliente-product.mjs` | Libs mínimas de un cliente nuevo |
| `new-domain.mjs` | Stub hexagonal backend en `@base/backend` |
| [add-frontend-domain.md](./add-frontend-domain.md) | Receta manual |
| [nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md) | Producto cliente completo |
