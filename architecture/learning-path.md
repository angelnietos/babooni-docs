# Ruta de aprendizaje — de junior a quien diseña el motor

Cuándo usarla: onboarding (día 1–30) o cuando “no sé por dónde empezar”.

Complementa [getting-started.md](../getting-started.md) (instalar) con **entender**.

---

## Mapa rápido

| Nivel | Tiempo | Meta |
|-------|--------|------|
| **L0** Setup | 0.5–1 día | Repo corre en local |
| **L1** Mapa | 1–2 días | Sabes dónde va cada cosa |
| **L2** Un dominio | 3–5 días | Lees y cambias un dominio E2E |
| **L3** Producto | 1–2 sem | Extiendes Josanz/SaaS sin romper capas |
| **L4** Platform | continuo | ADRs, gates, seams AI, visión |

---

## L0 — Setup (no te saltes esto)

1. [getting-started.md](../getting-started.md)
2. [guides/local-development.md](../guides/local-development.md) — puertos, Keycloak, infra opcional
3. Arranca `josanz-api` + SPA; login demo; una pantalla Clients

**Check:** puedes explicar qué puerto es API vs SPA vs Keycloak.

---

## L1 — Mapa mental (junior)

Lee en este orden:

1. [platform-vision.md](./platform-vision.md) §§1–3 — *para qué existe el motor*
2. [overview.md](./overview.md) §§0–4 — capas npm, apps vs libs, hex, 4 capas FE
3. [framework-decision-guide.md](./framework-decision-guide.md) — Nest / Angular / React / Next / RN
4. [overview.md](./overview.md) §9 — árbol “¿dónde va mi cambio?”
5. Ojeada a [../arquetipos/catalog.md](../arquetipos/catalog.md) — mapa de plantillas

**Analogía:** `@base` es el motor del coche; `@josanz` es la carrocería de un cliente;
`@arquetipos` es el kit de exposición del concesionario (no lo montas dentro del coche del cliente).

**Check:** dibujas de memoria el diagrama `producto → @base` y dices por qué Josanz no importa Arquetipos.

---

## L2 — Un dominio de punta a punta

Elige **clients** (dominio de referencia).

1. [domain-lifecycle.md](./domain-lifecycle.md) — request HTTP → CQRS → Prisma → FE
2. [backend-deep-dive.md](./backend-deep-dive.md) — carpetas hex + CQRS
3. [frontend-deep-dive.md](./frontend-deep-dive.md) — api / data-access / features / shell / ui
4. Código: `libs/base/backend/src/lib/domains/clients/` + `@base/clients-*` FE
5. Tests: [testing-pyramid.md](../guides/testing-pyramid.md) + un `*.spec.ts` de handler

**Ejercicio:** cambia un campo del listado (DTO → mapper → columna UI) sin saltarte capas.

**Check:** nombras el archivo concreto de Command, Handler, Controller, Service FE y Page.

---

## L3 — Producto y marca (mid)

1. [guides/add-frontend-domain.md](../guides/add-frontend-domain.md) + [add-backend-domain.md](../guides/add-backend-domain.md)
2. [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md) + [design-system.md](../frontend/design-system.md)
3. [josanz-product-exceptions.md](../frontend/josanz-product-exceptions.md)
4. [../arquetipos/how-to-use.md](../arquetipos/how-to-use.md) — copiar plantilla sin clonar Facebook
5. ADR [0008](../adr/adr-0008-platform-scope-vs-mvp-client.md) — MVP vs opt-in
6. [framework-decision-guide.md](./framework-decision-guide.md)

**Check:** decides sin dudar: ¿esto va en `@base` o en `@josanz`? ¿re-export o wrapper?

---

## L4 — Platform / senior / IA

1. ADRs 0001–0009 ([adr/README.md](../adr/README.md))
2. [ai-cqrs-policy.md](../guides/ai-cqrs-policy.md) — seam para agentes
3. [platform-vision.md](./platform-vision.md) §§3–6 — especialistas por dominio y SaaS
4. Scripts: `check-domain-conventions`, `check-lib-layout`, `check-ui-ownership`
5. [pr-checklist.md](../guides/pr-checklist.md) + [runbooks/](../runbooks/README.md)

**Check:** puedes proponer un ADR o un allow-list de command AI sin romper tenancy/auth.

---

## Glosario mínimo (léelo cuando te pierdas)

| Término | Significado aquí |
|---------|------------------|
| **Dominio** | Bounded context (`clients`, `billing`…) = carpeta + slug HTTP |
| **Hexagonal** | Núcleo de negocio aislado; IO en adapters |
| **Puerto** | Interface (p. ej. `ClientsRepository`) |
| **Adapter** | Implementación (Prisma, HTTP controller) |
| **CQRS** | Commands (write) / Queries (read) separados |
| **Facade / Service** | Clase que solo hace `commandBus.execute` / `queryBus.execute` |
| **Composition root** | App que **elige** módulos y env (no lógica de negocio) |
| **Thin lib** | Re-export de `@base` para poder overridear después |
| **Shell** | Entrada de rutas; lazy-load de features |
| **Outbox** | Evento persistido en la misma TX que el write → relay Kafka |
| **Tenant** | Organización; filtro `tenantId` en multi |
| **AI registry** | Mapa nombre → Query/Command expuesto a agentes |

---

## Anti-patrones de aprendizaje

- Leer solo planes históricos (`docs/plans/`) como si fueran la biblia.
- Empezar por microservicios / MF / RN el día 1 (opt-in).
- Copiar un dominio entero a producto en vez de extender facade/token.
- Pedir a la IA “haz un módulo” sin apuntarle a [add-backend-domain](../guides/add-backend-domain.md).

---

## Enlaces

- [overview.md](./overview.md)
- [platform-vision.md](./platform-vision.md)
- Hub: [docs/README.md](../README.md)
- Estilo docs: [CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md)
- Planes (no son biblia): [plans/README.md](../plans/README.md)
