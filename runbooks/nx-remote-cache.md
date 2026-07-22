<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Nx remote cache (Nx Replay)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Share task cache hits across developers and CI so lint, typecheck, test, and build
runs reuse prior results instead of redoing work.

> Local graph / daemon issues: [nx-daemon.md](./nx-daemon.md)  
> CI secrets policy: [secrets.md](./secrets.md)

---

## Current state (checklist)

| Item | Where | Status |
|------|-------|--------|
| Task caching enabled | `nx.json` → `targetDefaults.*.cache` | ✅ |
| Workspace linked to Nx Cloud | `nx.json` → `nxCloudId` | ⬜ run `pnpm exec nx connect` once |
| CI read/write token | GitHub secret `NX_CLOUD_ACCESS_TOKEN` | ⬜ add in repo Settings → Secrets |
| CI workflow wired | `.github/workflows/ci.yml` | ✅ (`NX_CLOUD_ACCESS_TOKEN` env) |
| Affected base SHA | `nrwl/nx-set-shas@v4` in CI jobs | ✅ |

Until `nxCloudId` and the CI token exist, Nx uses **local cache only** (`.nx/cache`,
gitignored). That is expected and safe.

---

## One-time setup (maintainer)

### 1. Connect the workspace

From the repo root (requires browser login to Nx Cloud):

```bash
pnpm exec nx connect
```

This adds a public `nxCloudId` to `nx.json`. **Commit that field** — it is not a secret.

Do **not** commit `nxCloudAccessToken` (legacy). Nx 19+ uses `nxCloudId` plus tokens
via env / `nx login`.

### 2. Create the CI access token

1. Open [Nx Cloud](https://cloud.nx.app) → your workspace → **Access tokens**.
2. Create a **read-write** token for CI (main/protected branches).
3. Optionally create a **read-only** token for PR workflows (see [Nx access tokens](https://nx.dev/docs/guides/nx-cloud/access-tokens)).

### 3. Add GitHub secret

Repository → **Settings → Secrets and variables → Actions**:

| Secret | Value |
|--------|--------|
| `NX_CLOUD_ACCESS_TOKEN` | CI read-write token (or read-only on PRs via GitHub Environments) |

Never commit token values. Optional local override: `nx-cloud.env` (gitignored).

### 4. Verify

```bash
# After connect + secret on CI, a clean run should show cache hits on unchanged tasks
pnpm verify:affected
```

In CI logs, look for `Nx Cloud` / cache hit lines instead of `NX_CLOUD disabled` or
`Remote cache disabled`.

---

## Developer daily use

After the workspace is connected:

```bash
# Read CI cache locally (one-time per machine)
pnpm exec nx login

pnpm verify:affected
pnpm typecheck:affected
pnpm nx typecheck <project>
```

Keep `NX_DAEMON=false` when the graph feels stuck ([nx-daemon.md](./nx-daemon.md)).
Remote cache works with or without the daemon.

---

## Agents and scripts

- **Do not** pass `--skip-nx-cache` / `--skipNxCache` unless you are debugging cache
  correctness. Skipping cache triggers Nx’s “enable remote cache” performance tip and
  slows every run.
- Legacy **raw `tsc` walkers** (`tools/typecheck/typecheck-affected.mjs`, `run-tsc.mjs`)
  set `NX_CLOUD=false` because they bypass Nx entirely — prefer `pnpm typecheck:affected`.
- Plans that mention `--skip-nx-cache` for **benchmarking** only; normal gates should
  use cached Nx targets.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| “Enable remote cache” tip every run | No `nxCloudId` or cold local cache | Run `nx connect`; run task twice locally |
| CI never hits remote cache | Missing `NX_CLOUD_ACCESS_TOKEN` secret | Add secret; re-run workflow |
| `monitor-ci` exits “Nx Cloud not connected” | `nx.json` lacks `nxCloudId` | Complete `nx connect` |
| Stale task output | Wrong `inputs` / `outputs` on target | Fix `project.json` / `nx.json`; see [Nx caching](https://nx.dev/concepts/how-caching-works) |

Remote cache falls back to local `.nx/cache` if Nx Cloud is unreachable.

---

## Optional: Nx Agents / distribution

This repo does **not** enable distributed execution yet. To add it later (after connect):

1. Uncomment / add `pnpm exec nx start-ci-run …` early in CI (see [Nx CI setup](https://nx.dev/docs/guides/nx-cloud/setup-ci)).
2. Tune agent count and stop conditions for cost vs speed.
