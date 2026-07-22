<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Runbook — Deploy & rollback</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## GitOps (ArgoCD) — preferido

Apps en `deploy/argocd/josanz-app.yaml` (dev / staging / prod), tracking Helm
`deploy/helm/josanz` con `valueFiles` y `targetRevision` por entorno.

- **Sync**: ArgoCD reconcilia al push. Estado:
  ```bash
  argocd app get josanz-prod
  argocd app sync josanz-prod
  ```
- **Rollback**:
  ```bash
  argocd app history josanz-prod
  argocd app rollback josanz-prod <revision>
  ```

## Helm manual — fallback

```bash
helm upgrade --install josanz deploy/helm/josanz \
  -n josanz -f deploy/helm/josanz/values.yaml -f deploy/helm/josanz/values-prod.yaml \
  --set image.tag=<SHA>

helm history josanz -n josanz
helm rollback josanz <revision> -n josanz
```

Nunca `latest` en prod — digest/SHA inmutable.

## Pre-flight

1. Image digest escaneado e inmutable.
2. `helm template -f values-prod.yaml` + `helm lint` verdes.
3. `prisma migrate deploy` **antes** de que los pods nuevos sirvan tráfico.
4. Secrets presentes (`kubectl -n josanz get secret josanz-secrets`).
5. Paridad schema si hubo cambio Prisma (`pnpm check:schema-parity` en CI).

## Post-deploy

- `kubectl -n josanz rollout status deploy/josanz`
- `GET /api/health/ready` → `200` ready o `degraded` si dep opcional caída
- 10 min: alertas `ApiHigh5xxRate` / `OutboxBacklogGrowing`
- Logs JSON con `requestId` / `tenantId` ([observability.md](./observability.md))

## Rollback rápido

1. ArgoCD/Helm rollback a revisión anterior.
2. Si migrate no es backward-compatible: restore según [backup-restore-pii.md](./backup-restore-pii.md) (coordina con datos).
3. Comunicar degraded mode si Kafka/Redis afectados ([kafka-redis-outage.md](./kafka-redis-outage.md)).

## Enlaces

- [secrets.md](./secrets.md)
- [database-migrations.md](./database-migrations.md)
- [scaling.md](./scaling.md)
