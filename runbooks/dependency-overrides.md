# Dependency overrides & upgrade policy (F36-P2-B)

## Current `pnpm.overrides` (root `package.json`)

| Override | Pinned to | Why |
|----------|-----------|-----|
| `@angular/platform-browser` | `21.2.17` | Align peer graph across Ionic + Nest/Storybook transitive Angular packages that drift to `21.2.18` |
| `@angular/platform-browser-dynamic` | `21.2.17` | Same — avoid dual Angular minor installs |
| `zone.js` | `0.15.1` | Required by Angular 21; pin so Expo/Ionic/Storybook do not pull incompatible zone builds |

These are **cutting-edge stack pins**, not a substitute for upgrading the workspace Angular/Nx versions.

## When to add an override

Add a `pnpm.overrides` entry only when:

1. `pnpm install` reports **unmet peers** that break build/test, **and**
2. The conflicting packages cannot be aligned by bumping the **declared** dependency in the owning app/lib, **and**
3. The pin is temporary (tracked) until the next coordinated stack upgrade.

Do **not** add overrides to silence warnings that do not affect runtime (e.g. RN peer `react@18` while the workspace is on React 19 for web — document instead).

## When to upgrade the stack

| Cadence | Scope |
|---------|--------|
| Security / CVE | Patch immediately (Angular, Nest, Prisma, Next, Expo) |
| Minor Nx / Angular | Batch in a dedicated PR after smoke (`typecheck:affected`, canary apps) |
| Major (Nx 24, Angular 22, …) | Plan round + ADR if it changes generator/plugin contracts |

**This round (F36):** no mass bump. Overrides stay documented; remove an override only when the declared deps already align and `pnpm install` peers are clean.

## Related

- [pnpm-layout.md](./pnpm-layout.md) — workspace linker / nested `node_modules`
- [nx-daemon.md](./nx-daemon.md) — large-graph DX
