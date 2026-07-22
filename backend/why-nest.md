<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Por qué NestJS (y no Express “a pelo”)</h1>

<p align="center">
  <img alt="backend" src="https://img.shields.io/badge/backend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: onboarding backend, debates de stack, o cuando alguien pregunte
“¿por qué no Express como en otros repos de la empresa?”.

---

## 1. Decisión de plataforma

| | **Express** | **NestJS (este monorepo)** |
|---|-------------|----------------------------|
| Estilo | Unopinionated — tú montas todo | Opinionated — DI, módulos, guards, pipes |
| Arquitectura | La inventas en cada proyecto | Hex + CQRS ya cableados en `@base/backend` |
| Comunidad | Enorme (downloads) | Más joven, creciendo; Angular-like |
| Coste de “hacerlo bien” | Alto (middlewares, estructura, testing) | Más bajo una vez el motor existe |
| Riesgo histórico | Arquitecturas flojas / inconsistentes entre repos | Un solo patrón gobernado por gates |

**Decisión:** el motor usa **Nest**. Express sigue siendo válido en el mercado;
aquí el valor es **gobernanza común**, no reinventar DI, módulos y capas en cada CRM.

---

## 2. Qué nos da Nest “de serie” que Express no

1. **Inyección de dependencias** — puertos/adapters se mockean en tests sin magia.
2. **Módulos** — cada dominio (`ClientsModule`) es un borde claro.
3. **Guards / Interceptors / Pipes** — auth JWKS, tenant, audit actor, validación DTO.
4. **CQRS** (`@nestjs/cqrs`) — CommandBus/QueryBus = seam para **IA read-only**
   ([ai-cqrs-policy](../guides/ai-cqrs-policy.md)).
5. **OpenAPI / tipado** — alineado con DTOs class-validator compartidos.

Con Express puro tendrías que construir (y mantener) todo eso en cada producto —
precisamente el dolor que este monorepo quiere eliminar.

---

## 3. Analogía Express / Nest ≈ React / Next (con matices)

En conversaciones de equipo a veces se dice:

> Express es flexible como “montar tú el routing”; Nest es framework vitaminado.

Es útil como **intuición**, pero:

- Nest **no** es un “Express con SSR”; es un framework de **aplicación servidor**
  con arquitectura.
- El paralelismo real: **sin estructura (Express/React a pelo) vs con opiniones
  (Nest / Next / Angular)**.
- Elegimos opiniones en backend porque los productos legacy con Express ya
  demostraron que “libertad” ≠ “calidad homogénea”.

---

## 4. Qué sigue siendo responsabilidad nuestra (no magia Nest)

Nest no sustituye:

- Diseño hexagonal y slugs ([backend-domain-convention](./backend-domain-convention.md))
- Prisma dual tenancy (ADR 0002)
- Outbox / Kafka (ADR 0004)
- Políticas de producto y AI allow-lists
- Composition root en `apps/*/backend` (qué módulos montar, qué `DATABASE_URL`)

---

## 5. Microservicios y gateway

Nest encaja con:

- Monolito (`josanz-api`, `api`, `api-single`)
- Microservicio (`clients-ms`) reutilizando el **mismo** `ClientsModule`
- Gateway (`api-gateway`) sin Prisma

Ver arquetipos backend: [../arquetipos/backend-apps.md](../arquetipos/backend-apps.md).

---

## 6. FAQ corta

**¿Podemos exponer un adapter Express dentro de Nest?**  
Sí (Nest puede montar Express debajo), pero **no** es el patrón de dominio.
Los dominios nuevos van hex + CQRS.

**¿Y si un cliente exige Express?**  
Producto excepcional fuera del motor, o BFF mínimo — no forks del kernel.

**¿ / cambiar Nest?**  
Cualquier cambio es ADR nuevo que supersede esta decisión. Hasta entonces,
Nest es ley de plataforma.

---

## Enlaces

- [how-it-works.md](./how-it-works.md)
- [../architecture/backend-deep-dive.md](../architecture/backend-deep-dive.md)
- ADR [0001](../adr/adr-0001-hexagonal-architecture.md) / [0009](../adr/adr-0009-cqrs-nest.md)
