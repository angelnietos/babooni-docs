<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Cómo usar un arquetipo como modelo de app nueva</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: kickoff — “tenemos que montar el CRM X / la app Y”.

---

## 1. Pasos (obligatorio en este orden)

### Paso 0 — Decidir stack (no abrir VS Code aún)

Usa [../architecture/framework-decision-guide.md](../architecture/framework-decision-guide.md):

- Backend: Nest monolito (default) ¿+ MS?
- FE: Angular SPA / React SPA / Next / ¿mobile?

Anota la decisión en el README del producto (una tabla de 5 líneas basta).

### Paso 1 — Localizar el arquetipo gemelo

Abre [catalog.md](./catalog.md) y elige **una** app FE + **una** app BE.

Ejemplos:

| Quieres | Copia modelo de |
|---------|-----------------|
| ERP Angular single-tenant | `angular-single` + `api-single` |
| Backoffice React multi | `react-multi` + `api` |
| Web con SSR | `next-single` + monolito API |
| App móvil Expo | `react-native-single` (+ API existente) |

### Paso 2 — Arrancar el arquetipo y validar

```bash
pnpm dev:infra          # si hace falta
pnpm nx serve <app-be>
pnpm nx serve <app-fe>
```

Login demo / health OK. Si está roto: **para** y arregla el arquetipo (es deuda del motor).

Smoke e2e canónico (Keycloak): `pnpm arq:fe:e2e:smoke` — matriz en [parity.md](./parity.md).
FE-only sin API: `pnpm mockserver` + `pnpm arq:fe:<app>:mock` ([mockserver.md](../runbooks/mockserver.md)).

### Paso 3 — Scaffold producto (no copy-paste gigante)

Preferido:

```bash
# ver scripts actuales en guides/README
pnpm exec node tools/scaffolds/scaffold-cliente-product.mjs …
```

Checklist: [../clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md).

El scaffold debe parecerse al arquetipo en **forma** (composition root, shells),
no duplicar todos los stacks.

### Paso 4 — Cablear dominios

1. Reutilizar `@base/*` (o thin `@arquetipos` solo si la plantilla aporta UX).
2. Dominios de producto en `@acme/*` / `@josanz/*`.
3. Misma regla de 4 capas FE + hex BE.

Walkthrough: [../guides/new-product-e2e-walkthrough.md](../guides/new-product-e2e-walkthrough.md).

### Paso 5 — Quitar lo que no usas

- No dejes MF/Next/RN “por si acaso”.
- No copies `libs/arquetipos` enteras al producto sin necesidad.
- Menú y rutas solo de dominios activos.

---

## 2. Qué copiar vs qué no

| Copiar / mirar | No copiar |
|----------------|-----------|
| Estructura `AppModule` / `app.routes` | Todo el árbol `apps/arquetipos` |
| Patrón env `bootstrap-env.ts` | Hardcode de puertos de demo como ley de prod |
| Thin override en un dominio | Fork de `@base/backend` |
| Tests e2e smoke como idea | Specs de plantilla sin adaptar |
| UX de login/layout si encaja | Marca Arquetipos en producto cliente |

---

## 3. Relación libs `@arquetipos/*`

Las apps importan `@arquetipos/angular-*-shell` etc., que **re-exportan** `@base/*`
hasta que alguien overridea.

Producto cliente serio:

- Preferir `@josanz/*` o `@acme/*` sobre base.
- Usar `@arquetipos` solo como referencia o si el producto *es* una plantilla.

Detalle: [../frontend/arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md).

---

## 4. Para la IA / agentes

Prompt útil:

> Usa como modelo la app `angular-single` y el API `api-single`. Respeta capas
> en docs/architecture y guides/add-*-domain. No generes Vue ni Express.
> No copies MF ni RN.

La biblia + este doc evitan que el agente monte Facebook el fin de semana.

---

## 5. Checklist de salida

- [ ] Stack decidido y escrito
- [ ] Arquetipo gemelo arranca en local
- [ ] Producto scaffold con 1 FE + 1 BE
- [ ] Un dominio E2E (list + create) verde
- [ ] `pr-checklist` / typecheck affected OK
- [ ] Sin stacks opt-in no pedidos

---

## Enlaces

- [catalog.md](./catalog.md)
- [../architecture/framework-decision-guide.md](../architecture/framework-decision-guide.md)
- [../architecture/platform-vision.md](../architecture/platform-vision.md)
