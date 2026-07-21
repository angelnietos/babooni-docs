# F56-C1 — MockServer global (fixtures + CLI)

## Estado

listo para ejecutar

## Objetivo

Entregar un **MockServer de workspace** (HTTP) con fixtures de los dominios
canario (`/api/auth`, `/api/clients`, `/api/users`, `/api/audit`, …) para que
cualquier front (arquetipos primero) pueda desarrollarse **sin** api-gateway,
Nest ni Postgres.

## Decisión de stack (elegir en Resultado)

| Opción | Pros | Contras |
|--------|------|---------|
| **Node + Fastify/Express + fixtures JSON** (recomendado) | Ligero, TS en repo, sin Java | Hay que mantener rutas a mano |
| MockServer (Java) | DSL potente | Pesado / Docker extra |
| MSW only (browser) | Cero puerto | No sirve a proxy Angular/Vite ni mobile |

Preferencia: **servidor Node en `tools/mockserver/`** (o `libs/base/.../mock-api`)
+ fixtures versionadas. MSW opcional después para unit tests FE.

## Tareas

1. Scaffold `tools/mockserver/` (package privado o script root):
   - `pnpm mockserver` → escucha p. ej. `:4010`
   - Health `GET /health`
   - Rutas `/api/{domain}` alineadas al slug HTTP del monorepo
2. Fixtures mínimas (JSON o TS) para login demo, list clients vacío/paginado,
   users, audit smoke.
3. Auth stub: endpoints que el FE de plantilla ya llama (cookies/JWT fake si
   aplica) — documentar contrato.
4. README corto + runbook `docs/runbooks/mockserver.md`.
5. No importar `@arquetipos/*` desde tools; fixtures → shapes de `@base/shared`
   cuando sea práctico.
6. Tests smoke del mock (supertest/jest) opcionales.

## Criterios de aceptación

- [ ] `pnpm mockserver` (o nombre acordado) arranca y responde `/health` + ≥2
      rutas `/api/*`.
- [ ] Runbook con puertos y cómo apuntar el proxy FE.
- [ ] Sin requerir Docker/Postgres para el mock.
