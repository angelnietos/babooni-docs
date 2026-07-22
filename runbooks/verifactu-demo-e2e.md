<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Runbook — Demo Verifactu end-to-end (F13-E2E)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Reproducible demo of **CRM API `:3120` + platform `:4230` + worker `:3130`** with a completed BullMQ queue.

Precondition: [guides/local-development.md](../guides/local-development.md) — Redis + Postgres CRM + seed.

---

## 1. Infra mínima (Postgres + Redis)

- [ ] Docker Desktop running
- [ ] `postgres` + `redis` healthy

```powershell
pnpm dev:infra:minimal        # docker compose up -d postgres redis
docker compose -f docker-compose.dev.yml ps
```

- [ ] DB `generic_crm` existe (one-shot)

```powershell
docker exec $(docker compose -f docker-compose.dev.yml ps -q postgres) `
  psql -U arquetipos -d arquetipos -c "CREATE DATABASE generic_crm;"
```

- [ ] CRM generado + migrado + sembrado

```powershell
$env:DATABASE_URL="postgresql://arquetipos:arquetipos@localhost:5440/generic_crm?schema=public"
pnpm saas:crm:generate
pnpm saas:crm:migrate
pnpm saas:crm:seed
```

> Seed demo tenants (password `Demo12345!`): `admin@josanz.com` (tenant `josanz`), `admin@demo.local`, `admin@acme.local`, `admin@alexis.local`, `admin@babooni.local`.

---

## 2. Smokes automatizados (sin levantar API/worker en foreground)

- [ ] `pnpm saas:infra:preflight` → all checks OK
- [ ] `pnpm saas:worker:smoke` → PASS (Redis `:6381`)

```powershell
pnpm saas:infra:preflight
pnpm saas:worker:smoke
```

Cola completa (requiere los 3 procesos de la sección 3):

```powershell
VERIFACTU_SMOKE_API=1 pnpm saas:verifactu:smoke
```

> `smoke-verifactu-api-health.mjs` y `smoke-verifactu-queue-e2e.mjs` también se pueden ejecutar a mano contra `VERIFACTU_API_URL` (default `:3120`).

---

## 3. Procesos (3 terminales)

- [ ] `pnpm saas:verifactu-crm-api` → log `http://localhost:3120/api` (docs `/api/docs`)
- [ ] `pnpm saas:verifactu-worker`  → log `Verifactu BullMQ worker initialized`
- [ ] `pnpm saas:verifactu-platform` → UI `:4230`

Copia `.env.example` → `.env` en `apps/productos-saas/verifactu-crm-api` y `verifactu-worker` (comparten vars CRM). Worker usa `REDIS_PORT=6381`; API usa `ERP_API_URL=http://localhost:3130/api` (worker como cola canónica en dev).

---

## 4. API

- [ ] `GET http://localhost:3120/api/docs` → Swagger UI
- [ ] Login demo: `admin@josanz.com` / `Demo12345!` / tenant `josanz`
- [ ] `POST /api/verifactu/queue` con `invoiceId` seed → status `COMPLETED` en ~3s

```powershell
$login = Invoke-RestMethod -Uri http://localhost:3120/api/auth/login -Method POST `
  -Body '{"email":"admin@josanz.com","password":"Demo12345!","tenantSlug":"josanz"}' `
  -ContentType application/json
$h = @{ Authorization = "Bearer $($login.accessToken)"; 'x-tenant-id' = $login.tenantId }
Invoke-RestMethod -Uri http://localhost:3120/api/verifactu/queue -Method POST -Headers $h `
  -Body '{"invoiceId":"f1a2b3c4-d5e6-4789-a012-345678901002"}' -ContentType application/json
# → en ~3s GET /api/verifactu/queue muestra status COMPLETED para esa factura
```

---

## 5. UI (Playwright o manual)

- [ ] `/` — shell CRM / inicio
- [ ] `/invoices` — listado facturas
- [ ] `/verifactu/overview` — resumen Verifactu
- [ ] `/verifactu/queue` — cola AEAT
- [ ] Lazy shells: `/clients`, `/identity` (si rutas expuestas en `app.routes.ts`)

```powershell
pnpm test:smoke:verifactu-platform   # Playwright contra :4230
```

---

## Puertos de referencia

| Servicio | Puerto |
|----------|--------|
| Postgres | 5440 |
| Redis | 6381 |
| CRM API | 3120 |
| Worker | 3130 |
| Platform UI | 4230 |
