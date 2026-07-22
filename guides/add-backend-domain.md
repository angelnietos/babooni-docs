<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Añadir un dominio backend</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


Receta para exponer un nuevo bounded context vía NestJS. El slug debe alinearse con frontend y HTTP.

---

## ¿Kernel, producto o SaaS?

| Caso | Dónde | Ejemplo |
|------|-------|---------|
| Dominio reusable para todos | `@base/backend` hex | `clients`, `billing` |
| Reglas solo de un cliente | `@josanz/backend` `src/lib/{domain}/` | `fleet`, `staff` |
| Dominio CRM Verifactu | `@saas/{domain}-backend` | `invoicing` |

**Slug único:** `{domain}` en `@scope/{domain}-*`, `@Controller('{domain}')`, carpeta lib.

Convención detallada: [backend-domain-convention.md](../backend/backend-domain-convention.md).

---

## Patrón producto Josanz (canónico)

### 1. Estructura en lib

```
libs/clientes/josanz/backend/src/lib/{domain}/
├── domain/
│   ├── {entity}.types.ts
│   └── {entity}.repository.port.ts    # símbolo FOO_REPOSITORY
├── infrastructure/
│   └── prisma-{entity}.repository.ts  # adaptador
├── {domain}.controller.ts
├── {domain}.service.ts               # inyecta puerto, no PRISMA_SERVICE
├── {domain}.dto.ts
└── josanz-{domain}.module.ts
```

**Por qué puertos:** la app (o un futuro microservicio) puede cambiar adaptador o BD sin tocar el servicio. Piloto: `JosanzFleetModule`.

### 2. Módulo Nest

```typescript
@Module({
  imports: [PrismaModule],
  controllers: [JosanzOrdersController],
  providers: [
    PrismaOrdersRepository,
    { provide: ORDERS_REPOSITORY, useExisting: PrismaOrdersRepository },
    JosanzOrdersService,
  ],
  exports: [JosanzOrdersService],
})
export class JosanzOrdersModule {}
```

### 3. Registrar en app

`apps/clientes/josanz/backend/src/app/app.module.ts`:

```typescript
imports: [
  // …
  JosanzOrdersModule,
],
```

### 4. Export en barrel

`libs/clientes/josanz/backend/src/index.ts` → `export { JosanzOrdersModule } from './lib/orders/josanz-orders.module';`

### 5. DTOs

Tipos compartidos en `@josanz/shared` si el frontend los consume.

---

## Patrón kernel `@base/backend` (hex)

Para dominios que deben vivir en monolito **y** microservicios:

```bash
node tools/scaffolds/new-domain.mjs orders --dry
node tools/scaffolds/new-domain.mjs orders
node tools/checks/check-domain-conventions.mjs
```

Layout (F33 — Nest CQRS):

```
hex/domain/orders/          # entity, repository port, errors, mappers, index
hex/application/orders/
  commands/ | queries/ | handlers/
  orders.service.ts         # facade → CommandBus / QueryBus
hex/infrastructure/orders/
  orders.module.ts | orders.controller.ts | orders.prisma.repository.ts
```

`ClientsModule` es la referencia completa (handlers Nest CQRS, eventos Kafka,
`REPOSITORY_TOKENS`, `AI_QUERY_CONTRIBUTION`). Scaffold: `new-domain.mjs`.
Validación: `pnpm check:domain-conventions:strict`.

---

## Base de datos

- La **lib** usa `PRISMA_SERVICE` solo en el **adaptador** infrastructure.
- La **app** fija la URL en `bootstrap-env.ts` — no en la lib.
- Migraciones: schema `single` (Josanz) o `multi` (plantillas) — [database-migrations.md](../runbooks/database-migrations.md).

---

## Verificación

```bash
npx tsc -p libs/clientes/josanz/backend/tsconfig.lib.json --noEmit
npx jest --config libs/base/backend/jest.config.ts   # si tocaste hex
pnpm test:smoke:backend                             # smoke dominios base
```

Añade tests en `{service}.spec.ts` o use-case specs en hex.

---

## Enlaces

- [add-microservice.md](./add-microservice.md) — si el dominio sale a proceso propio
- [add-frontend-domain.md](./add-frontend-domain.md)
- [adr-0001](../adr/adr-0001-hexagonal-architecture.md)
