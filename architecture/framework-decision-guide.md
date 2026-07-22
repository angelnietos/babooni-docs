<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Guía de decisión de frameworks</h1>

<p align="center">
  <img alt="architecture" src="https://img.shields.io/badge/architecture-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: kickoff de producto, reunión de stack, o para **no repetir** el
debate React vs Next vs Angular vs RN / Nest vs Express.

Esta guía es **ley operativa del motor** hasta que un ADR la superseda.
Complementa [ADR 0008](../adr/adr-0008-platform-scope-vs-mvp-client.md) (MVP vs opt-in).

---

## 1. Objetivo

> Misma gobernanza y arquitectura de dominio en todos los proyectos. Elegimos
> **runtime** según necesidad real del cliente — no por moda ni por simetría
> con el catálogo de arquetipos.

Los arquetipos existen para **probar y copiar** el stack elegido
([../arquetipos/README.md](../arquetipos/README.md)), no para montar todos a la vez.

---

## 2. Backend

| Opción | ¿En el motor? | Cuándo |
|--------|---------------|--------|
| **NestJS + hex + CQRS** | **Sí — default** | Todo producto nuevo sobre este repo |
| Express “a pelo” | No como kernel | Legacy externo; no forks del motor |
| Microservicios Nest | Opt-in | Dominio con ciclo/escala propia ([add-microservice](../guides/add-microservice.md)) |

**Por qué Nest:** DI, módulos, guards, CQRS (seam AI), menos arquitectura “floja”
repetida. Detalle: [../backend/why-nest.md](../backend/why-nest.md).

---

## 3. Frontend web — árbol de decisión

```
¿App interna tipo CRM/ERP (auth, poco SEO)?
├─ ¿Equipo / producto ya Angular o preferencia Angular?
│     └─ SÍ → Angular SPA (arquetipo angular-single/multi)  ★ default ERP
├─ ¿Equipo React y NO necesitas SSR/SEO/middleware Next?
│     └─ React SPA (react-single/multi) — misma arquitectura 4 capas
└─ ¿Necesitas SSR, SEO, App Router, middleware, Image/Link optimizados de verdad?
      └─ Next.js (next-single/multi)

¿Marketing / web pública / SEO crítico?
└─ Next.js (o Angular Universal — no es el default del catálogo hoy)
```

### React SPA vs Next — criterio explícito

| Señal | Preferir |
|-------|----------|
| CRM/backoffice tras login; SEO irrelevante (ej. muchos módulos Josanz) | **React SPA** o **Angular SPA** |
| Landing, docs públicas, OG tags, SSR de verdad | **Next** |
| “Next vitaminado siempre” sin usar SSR/SSG/middleware | Evitar complejidad — usa SPA |
| Equipo solo Next en la empresa y el producto es web pública | Next |

Next **no** es obligatorio en CRMs. La ventaja (routing file-based, middleware,
optimizaciones) existe; si el producto **no las usa**, estás pagando superficie
sin beneficio. Documentad la elección en el README del producto.

Angular sigue siendo **primera clase** en este motor (Josanz ERP).

---

## 4. Mobile

| Opción | Default | Notas |
|--------|---------|-------|
| **Expo + React Native** | **Sí** para app nativa | Metro; estándar actual (no RN CLI salvo nativos duros) |
| Ionic | Híbrido / web-like | arquetipos ionic-single/multi |
| Solo web responsive | MVP web | No sustituye app si el cliente prioriza mobile |

Si el cliente dice “mobile > web” (caso Josanz): planifica arquetipo RN/Ionic;
no asumas que el SPA responsive cierra el contrato.

Guía: [add-mobile-domain](../guides/add-mobile-domain.md).

---

## 5. Module Federation

Solo si hay **requisito explícito** de host/remotes independientes (equipos,
deploys separados). No por “tener el arquetipo MF”.

---

## 6. UI compartida (Lit) — transversal a la elección FE

Independiente de Angular vs React vs Next:

- **SoT:** primitivos en `@base/native-ui` (Lit CE) — [ADR 0010](../adr/adr-0010-native-ui-lit-sot.md)
- Wrappers por framework (`Native*`) + marca; **freeze** de primitivos
  solo-framework nuevos en base ([ui-strategy](../frontend/ui-strategy.md) § freeze)
- Storybook: native primero — [ADR 0011](../adr/adr-0011-storybook-native-ui-first.md)
- RN: tokens/API, no Lit en native

Ver [../frontend/ui-strategy.md](../frontend/ui-strategy.md).

---

## 7. Matriz rápida producto nuevo

| Pregunta de kickoff | Respuesta tipo |
|---------------------|----------------|
| ¿Un solo FE el día 1? | **Sí** (ADR 0008) |
| ¿Cuál? | Angular SPA **o** React SPA **o** Next — según árbol §3 |
| ¿Backend? | Nest monolito + `@base/backend` |
| ¿Mobile día 1? | Solo si el contrato lo exige |
| ¿MF / MS? | Flags explícitos |
| ¿Modelo a copiar? | App bajo `apps/arquetipos/...` del stack elegido |

Checklist: [../clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md).

---

## 8. Gobernanza y IA

Esta guía existe para que:

1. Humanos no reabran el hilo Express/Nest o Next-siempre cada semana.
2. La IA genere código en el **stack ya elegido**, sobre capas fijas.
3. El equipo se centre en el **motor** y en producto — no en reinventar carpetas.

Visión: [platform-vision.md](./platform-vision.md).

---

## Enlaces

- [../arquetipos/README.md](../arquetipos/README.md) — catálogo vivo de plantillas
- [../backend/why-nest.md](../backend/why-nest.md)
- [../frontend/how-it-works.md](../frontend/how-it-works.md)
- ADR [0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)
