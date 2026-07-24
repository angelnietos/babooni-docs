'use client';

/**
 * Next.js + Lit native-ui (F79-B1)
 *
 * App Router Server Components cannot run Lit. SoT atoms in `@base/next-ui`
 * are `'use client'` islands that re-export / thin-wrap `@base/react-ui`
 * `Native*` adapters (which register `@base/native-ui` CEs).
 *
 * - Prefer `Arq*` public API (stable) → Native under the hood.
 * - Do not maintain a second CSS design for SoT atoms.
 * - Composites (`ArqPageHeader`, `ArqTable`, …) may stay markup; they compose atoms.
 *
 * See: [native-ui-adapter-matrix.md](./native-ui-adapter-matrix.md).
 */
export {};
