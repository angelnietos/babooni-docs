# Runbook — Deploy & rollback

## GitOps (ArgoCD) — preferred

Applications live in `deploy/argocd/josanz-app.yaml` (dev / staging / prod),
each tracking the Helm chart in `deploy/helm/josanz` with its own `valueFiles`
and `targetRevision`.

- **Sync**: ArgoCD auto-reconciles on git push. Check status:
  ```bash
  argocd app get josanz-prod
  argocd app sync josanz-prod        # force a sync
  ```
- **Rollback** (bad release):
  ```bash
  argocd app history josanz-prod
  argocd app rollback josanz-prod <revision>
  ```

## Manual Helm — fallback

```bash
# Set the immutable image digest (CI injects the SHA; never 'latest' in prod).
helm upgrade --install josanz deploy/helm/josanz \
  -n josanz -f deploy/helm/josanz/values.yaml -f deploy/helm/josanz/values-prod.yaml \
  --set image.tag=<SHA>

# Rollback
helm history josanz -n josanz
helm rollback josanz <revision> -n josanz
```

## Pre-flight

1. Image digest is immutable and scanned.
2. `helm template -f values-prod.yaml` renders cleanly (`helm lint` green).
3. DB migration applied (`prisma migrate deploy`) before the new pods serve.
4. Secrets present (`kubectl -n josanz get secret josanz-secrets`).

## Post-deploy

- `kubectl -n josanz rollout status deploy/josanz`
- Hit `GET /api/health/ready` → expect `200 { status: "ready" }` (or
  `degraded` if an optional dep is intentionally down).
- Watch `ApiHigh5xxRate` / `OutboxBacklogGrowing` alerts for the first 10m.
