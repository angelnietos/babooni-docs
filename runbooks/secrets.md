# Runbook — Secrets management

Secrets are **never** committed to git as plaintext. The repo references an
existing `Secret` named `josanz-secrets` (`.Values.existingSecretName` in the
Helm chart) whose values are injected out of band.

## Secret inventory

| Key | Source |
|-----|--------|
| `DATABASE_URL` | Database operator / Vault |
| `REDIS_HOST`, `REDIS_PORT` | Redis operator |
| `KAFKA_BROKERS` | Kafka operator |
| `KEYCLOAK_CLIENT_SECRET` | Keycloak / IdP |
| `APP_ENCRYPTION_KEYS` | KMS (see rotation below) |
| `APP_ENCRYPTION_KEY_ID` | KMS |

## Option A — SealedSecret (bitnami/sealed-secrets)

1. Encrypt locally with the cluster public key:
   ```bash
   kubeseal --format yaml < raw-secrets.yaml > sealed-secrets.yaml
   kubectl apply -f sealed-secrets.yaml   # creates Secret `josanz-secrets`
   ```
2. The chart renders the `SealedSecret` itself when `.Values.sealedSecret.enabled=true`
   (see `deploy/helm/josanz/templates/secrets.yaml`).

## Option B — ExternalSecrets / Vault

1. Create a `ClusterSecretStore` pointing at your secret manager.
2. Apply an `ExternalSecret` that writes `josanz-secrets`.
3. Leave `.Values.sealedSecret.enabled=false`; the Deployment already references
   the Secret by name.

## Rotation — KMS / field-encryption keys

1. Add the new key to `APP_ENCRYPTION_KEYS` (JSON map `keyId -> hex/base64`) and
   bump `APP_ENCRYPTION_KEY_ID`.
2. Run `KeyRotationService.rotateModel(...)` to re-encrypt existing PII under the
   new `kid`. Old envelopes keep their `kid` and remain decryptable.
3. Remove retired keys only after all ciphertext has been re-encrypted.

## CI

CI uses a `secretGenerator` step to mint the real `josanz-secrets` from the CI
secret store at release time — no secret value is ever written to the repo.

### GitHub Actions (repository secrets)

| Secret | Purpose |
|--------|---------|
| `NX_CLOUD_ACCESS_TOKEN` | Nx Cloud remote cache read/write in CI ([nx-remote-cache.md](./nx-remote-cache.md)) |

`nxCloudId` in `nx.json` is **public** and safe to commit; only the access token
stays in GitHub Secrets (or `nx-cloud.env` locally, gitignored).
