# Catálogo de apps Arquetipos

Inventario de proyectos Nx bajo `apps/arquetipos/`. Puertos: confirmar siempre en
`project.json` / [local-development](../guides/local-development.md).

---

## Backend

| Proyecto Nx | Path | Rol | Puerto típico |
|-------------|------|-----|---------------|
| `api` | `backend/monolith/api` | Monolito multi-tenant | 3000 |
| `api-single` | `backend/monolith/api-single` | Monolito single-tenant | 3000 |
| `api-gateway` | `backend/gateway/api-gateway` | Gateway (sin Prisma) | 4000 |
| `clients-ms` | `backend/microservices/clients-ms` | Microservicio Clients | gRPC 50051 / HTTP 4001 |

Detalle: [backend-apps.md](./backend-apps.md).

---

## Frontend web

| Proyecto Nx | Path | Rol | Puerto típico |
|-------------|------|-----|---------------|
| `angular-single` | `frontend/angular/single-tenant/angular-single` | Angular SPA single | 4200 |
| `angular-multi` | `frontend/angular/multi-tenant/angular-multi` | Angular SPA multi | 4203 |
| `react-single` | `frontend/react/single-tenant/react-single` | React SPA single | 4201 |
| `react-multi` | `frontend/react/multi-tenant/react-multi` | React SPA multi | 4202 |
| `next-single` | `frontend/nextjs/next-single` | Next.js single | 4240 |
| `next-multi` | `frontend/nextjs/next-multi` | Next.js multi | (ver project) |
| `mf-host-angular` | `frontend/angular/microfrontend/mf-host` | Host MF | ~4210 |
| `mf-remote-react` | `frontend/react/microfrontend/mf-remote-clients` | Remote clients | ~4211 |
| `mf-remote-audit` | `frontend/react/microfrontend/mf-remote-audit` | Remote audit | ~4212 |

Hay proyectos `*-e2e` Playwright asociados.

Detalle: [frontend-apps.md](./frontend-apps.md).

---

## Mobile

| Proyecto Nx | Path | Rol | Puerto web |
|-------------|------|-----|------------|
| `ionic-single` | `mobile/ionic-single` | Ionic single | 4300 |
| `ionic-multi` | `mobile/ionic-multi` | Ionic multi | 4301 |
| `react-native-single` | `mobile/react-native-single` | Expo RN web/native | 8091 |
| `react-native-multi` | `mobile/react-native-multi` | Expo RN multi | 8092 |

Detalle: [mobile-apps.md](./mobile-apps.md).

---

## Comandos útiles

```bash
pnpm nx show projects --projects "tag:layer:arquetipos"
pnpm nx serve angular-single
pnpm nx serve api-single
pnpm nx serve react-native-single
```

---

## Enlaces

- [how-to-use.md](./how-to-use.md)
- [../architecture/framework-decision-guide.md](../architecture/framework-decision-guide.md)
