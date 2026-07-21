# Arquetipos backend — monolito, gateway, microservicio

Cuándo usarla: elegir forma de despliegue API al copiar plantilla.

---

## 1. Monolito (`api` / `api-single`)

**Qué es:** un proceso Nest que importa muchos dominios `@base/backend` (+ thin arquetipos).

| App | Tenancy demo |
|-----|----------------|
| `api` | multi (`TENANT_MODE=multi`) |
| `api-single` | single |

**Sirve de modelo para:** `josanz-api` y cualquier producto MVP.

**Mirar en el código:**

- `AppModule` — lista de módulos
- `bootstrap-env.ts` — env dedicada → `DATABASE_URL`
- Health / auth wiring

No copies lógica de dominio aquí: solo composición.

---

## 2. Gateway (`api-gateway`)

**Qué es:** borde HTTP que enruta a monolito y/o gRPC `clients-ms`. **Sin** Prisma.

**Sirve de modelo para:** edge API cuando hay MS. Opt-in — no MVP día 1.

---

## 3. Microservicio (`clients-ms`)

**Qué es:** el mismo `ClientsModule` hexagonal expuesto por gRPC/HTTP propio,
con su `CLIENTS_MS_DATABASE_URL` (compartida o aislada).

**Sirve de modelo para:** extraer un dominio con ciclo de vida propio.

Guía: [../guides/add-microservice.md](../guides/add-microservice.md).  
BD: [../backend/backend-domain-convention.md](../backend/backend-domain-convention.md).

---

## 4. Cómo elegir

```
¿MVP producto?
└─ monolito api-single o api

¿Necesitas aislar Clients (escala/equipo)?
└─ clients-ms + gateway (opt-in)

¿Solo BFF sin BD?
└─ gateway pattern
```

---

## Enlaces

- [../backend/how-it-works.md](../backend/how-it-works.md)
- [catalog.md](./catalog.md)
