<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Josanz — excepciones de estructura (producto)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

**ID plan:** F7-S5 (F7-S5.ui, F7-S5.audit, F7-S5.users)  
**Ámbito:** `libs/clientes/josanz/`

El ERP Josanz sigue el patrón canónico de 4 capas en dominios de producto (`events`, `clients`, `staff`, …). Tres casos tienen **excepciones documentadas** respecto al árbol ideal `libs/clientes/josanz/frontend/angular/`.

---

## F7-S5.ui — UI en `angular-ui/` (raíz de producto)

| Decisión | **Excepción permanente** — no migrar a `frontend/angular/ui/` en F7-S5 |
|----------|--------------------------------------------------------------------------|

### Path actual

```
libs/clientes/josanz/
├── angular-ui/          → @josanz/angular-ui  (Storybook, Tailwind, branding Figma)
└── angular/
    └── {domain}/        → dominios 4 capas
```

### Motivo

- Librería grande (~400 componentes) con Storybook propio (`.storybook/`), tokens SCSS y `tailwind.config.js` referenciados desde `josanz` app.
- Mover a `frontend/angular/ui/` sería un refactor masivo (imports, `project.json`, paths Tailwind) sin beneficio funcional inmediato.
- El linter `check-lib-layout.mjs` registra la excepción `josanz-angular-ui-root`.

### Reglas de consumo

- Features Josanz importan **`@josanz/angular-ui`** para UI de marca.
- Genérico reutilizable → promover a `@base/angular-ui` (ver catálogo UI / F7-UI1).
- **No** importar `@arquetipos/*` desde clientes.

---

## F7-S5.audit — Audit thin 4 capas

| Decisión | **4 capas Nx** con re-export base (no lógica Josanz propia) |
|----------|-------------------------------------------------------------|

### Estructura

```
libs/clientes/josanz/angular/audit/
├── api/            → @josanz/audit-api           → AuditLogDto desde @base/shared
├── data-access/    → @josanz/audit-data-access   → auditStore desde @base/audit-data-access
├── shell/          → @josanz/audit-shell         → rutas lazy
└── features/       → @josanz/audit-features      → auditFeatureRoutes desde @base/audit-features
```

No hay reglas de negocio audit específicas Josanz: el dominio es **kernel compartido** expuesto con namespace `@josanz/audit-*` para consistencia con otros dominios del producto.

### App wiring

```typescript
// apps/clientes/josanz/frontend/josanz/src/app/app.routes.ts
{
  path: 'audit',
  loadChildren: () => import('@josanz/audit-shell').then((m) => m.auditShellRoutes),
}
```

`app.config.ts` registra `auditStore` desde `@base/audit-data-access` (mismo store que re-exporta `@josanz/audit-data-access`).

Excepción linter: `josanz-audit-2-layer` (histórico) — paths bajo `audit/` ignoran validación de capas incompletas.

---

## F7-S5.users — Sin libs `@josanz/users-*`

| Decisión | **Uso directo de `@base/users-*`** — no crear dominio Josanz |
|----------|--------------------------------------------------------------|

### Motivo

Users es dominio **kernel** idéntico en plantillas y ERP: guards, NgRx store y features viven en `@base/users-{data-access,shell,features}`. Duplicar thin libs Josanz aportaría solo aliases sin valor.

### App wiring

```typescript
// app.routes.ts
{
  path: 'users',
  loadChildren: () => import('@base/users-shell').then((m) => m.usersShellRoutes),
}

// app.config.ts
import { usersStore } from '@base/users-data-access';
provideState(usersStore.feature);
provideEffects(usersStore.Effects);
```

### Qué no hacer

- No añadir `libs/clientes/josanz/angular/users/` salvo requisito producto futuro (p. ej. columnas/permisos exclusivos Josanz).
- Features de otros dominios que necesiten tipos user → `@base/shared` o `@base/users-data-access`.

---

## F14-UI1 — Presentation theme (F14)

| Decisión | Tokens presentacionales en `@base/angular-ui`; Josanz los adapta vía `buildJosanzPresentationTheme()` |
|----------|-----------------------------------------------------------------------------------------------------|

### Base (`@base/angular-ui`)

- `PresentationThemeTokens` + `DEFAULT_PRESENTATION_THEME` en `lib/theme/presentation-theme.ts`
- Primitivos promovidos: `lib-presentation-badge`, `lib-presentation-alert`, `lib-presentation-empty-state`, `lib-presentation-block-editor` (`@base/angular-ui/document-editor`)

### Josanz (`@josanz/angular-ui`)

- Wrappers finos (`josanz-badge`, `josanz-alert`, `josanz-empty-state`) inyectan tokens `var(--josanz-*)`
- **Product-only:** botones de acción en empty-state/alert (`josanz-button`, `josanz-secondary-button`), spinner, layouts Figma

---

## F52-B1 — `@josanz/storybook` (tooling, no dominio)

| Decisión | Tooling Storybook en `libs/clientes/josanz/storybook` — **no** es dominio 4 capas |
|----------|-----------------------------------------------------------------------------------|

- Tags: `layer:clientes`, `runtime:angular`, `type:tooling`.
- Heurística `check-lib-layout`: path `…/clientes/*/storybook` → `runtime:angular`.
- Exporta helpers (`./arg-types`); no lazy-load de features.

---

## Comparativa rápida

| Caso | Path / paquete | Capas Josanz | Fuente de verdad |
|------|----------------|--------------|------------------|
| UI marca | `angular-ui/` | 1 lib UI | `@josanz/angular-ui` |
| Audit | `angular/audit/` | 4 thin | `@base/audit-*` |
| Users | *(ninguna lib Josanz)* | — | `@base/users-*` |
| Dominio producto (ej. events) | `angular/events/` | 4 completas | `@josanz/events-*` |

---

## Verificación

```bash
node tools/checks/check-lib-layout.mjs
npx tsc -p apps/clientes/josanz/frontend/josanz/tsconfig.app.json --noEmit
```

---

## Enlaces

- [AGENTS.md](../../AGENTS.md)
- [arquetipos-thin-libs.md](./arquetipos-thin-libs.md) — contraste plantillas vs producto
- [ui-component-catalog.yaml](./ui-component-catalog.yaml)
