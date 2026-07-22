<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0003: AES-256-GCM field encryption vs KMS</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted (with extension path to KMS)
- Date: 2026-07-08

## Context

PII columns (emails, names, passwords, client contacts) must be unreadable from
a raw DB dump. We must choose between application-layer envelope encryption and
a managed KMS.

## Decision

Use **AES-256-GCM field encryption at the application layer**, implemented as a
Prisma `query` extension (`security.extension.ts`) driven by a `KeyProvider`.
Deterministic encryption (HMAC-derived IV) keeps equality queries working on
login/lookup columns. Every ciphertext envelope carries a `kid` (key id).

The `KeyProvider` is pluggable: the default `EnvMasterKeyProvider` supports
**key rotation** via `APP_ENCRYPTION_KEYS` (map of `kid -> key`), and
`KmsKeyProvider` is a ready stub for AWS KMS envelope encryption. Decryption
always resolves the key by the `kid` stamped on the envelope, so rotated keys
keep decrypting old data.

## Consequences

- Works without external infrastructure; transparent to every repository.
- `kid` enables zero-downtime rotation via `KeyRotationService.rotateModel`.
- Application holds the key, so process memory compromise is a risk — mitigated
  by moving to `KmsKeyProvider` (per-record wrapped DEKs) when a KMS is
   available. The interface already supports that swap without code changes.

## See also

- `KeyProvider` / `EnvMasterKeyProvider` / `KmsKeyProvider`: `libs/base/backend/src/lib/security/key-provider.ts`.
- Re-encryption: `KeyRotationService.rotateModel` + the `rotate-pii` BullMQ job (see runbook [`runbooks/pii-key-rotation.md`](../runbooks/pii-key-rotation.md)).
- PII backup & restore runbook: `runbooks/backup-restore-pii.md` (the key **is** part of the data).
- Back to the [docs hub](../README.md).
