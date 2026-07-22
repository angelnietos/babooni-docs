<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Josanz billing ↔ Verifactu CRM (F7-B4.4b)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Integración técnica entre `josanz-api` y el stack SaaS Verifactu (`verifactu-crm-api` + `verifactu-worker`).

## Estado

| Aspecto | Estado |
|---------|--------|
| Código backend (`JosanzVerifactuBillingModule`) | ✅ Implementado |
| Prisma `VerifactuQueueItem` (ERP mirror) | ✅ Schema + migración |
| Frontend hook (`JosanzBillingVerifactuService`) | ✅ |
| **Sign-off producto/legal producción** | ⏸️ Pendiente |

La integración está **deshabilitada por defecto**. Activar solo en dev/staging hasta sign-off.

## Variables de entorno (josanz-api)

```env
# Opt-in explícito (default: false)
VERIFACTU_BILLING_ENABLED=false

# Producción: requiere sign-off documentado
# VERIFACTU_BILLING_SIGN_OFF=acknowledged

# CRM Verifactu (réplica facturas + cola UI)
VERIFACTU_CRM_API_URL=http://localhost:3120/api
CRM_ERP_SYNC_API_KEY=dev-crm-erp-sync

# Worker BullMQ (cola canónica; alternativa a apuntar ERP_API_URL al worker)
VERIFACTU_WORKER_API_URL=http://localhost:3130/api
```

En `verifactu-crm-api`, para modo worker ERP:

```env
VERIFACTU_USE_ERP_WORKER=true
ERP_API_URL=http://localhost:3000/api   # josanz-api O http://localhost:3130/api (worker directo)
CRM_ERP_SYNC_API_KEY=dev-crm-erp-sync
```

## Endpoints

| Método | Ruta | Auth | Descripción |
|--------|------|------|-------------|
| `GET` | `/api/billing/verifactu/integration` | Público | Estado + flags |
| `POST` | `/api/billing/:id/verifactu/submit` | JWT Admin/Manager | Envía billing a Verifactu |
| `POST` | `/api/internal/verifactu/enqueue` | `x-api-key` | CRM → ERP (worker mode) |

## Flujo

1. Billing con status `issued` / `paid` → `POST .../verifactu/submit`
2. Espejo en CRM (`/internal/erp/invoices/mirror`)
3. Encolado en worker (`/internal/verifactu/enqueue`) o CRM (`/verifactu/submit`)
4. Registro local en `VerifactuQueueItem` (josanz DB)

## Go / no-go producción

Antes de `VERIFACTU_BILLING_ENABLED=true` + `VERIFACTU_BILLING_SIGN_OFF=acknowledged` en producción:

- [ ] Aprobación legal AEAT / Verifactu
- [ ] Certificados mTLS por tenant en CRM
- [ ] Smoke: billing → cola → worker → `COMPLETED`
- [ ] Revisión DPO / retención logs fiscales

## Verificación local

```bash
npx tsc -p apps/clientes/josanz/backend/tsconfig.app.json --noEmit
# Con stack SaaS levantado (:3120, :3130, Redis):
# curl http://localhost:3000/api/billing/verifactu/integration
```
