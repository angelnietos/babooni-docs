# Ronda F57 — Cobertura real, scaffolds unificados y carry F56

## Estado

listo para ejecutar

> Abierta 2026-07-22. Prioridad: **verdad del informe de coverage** por
> app/lib + **scaffolding** usable; carry Chromatic/Code Connect/deprecated y
> TS6059 React features.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F57-A1 | [Coverage report truth (por proyecto)](1750000140000-f57-coverage-report-truth.md) | listo para ejecutar |
| F57-A2 | [Coverage inventory + CI soft→strict](1750000141000-f57-coverage-inventory-ci.md) | listo para ejecutar |
| F57-B1 | [Scaffolds unificados (CLI + README)](1750000142000-f57-scaffolds-unified-cli.md) | listo para ejecutar |
| F57-B2 | [Scaffolds: tags runtime + SaaS/React parity](1750000143000-f57-scaffolds-runtime-saas-react.md) | listo para ejecutar |
| F57-C1 | [Fix TS6059 arquetipos-react-*-features](1750000144000-f57-react-features-rootdir-tsc.md) | listo para ejecutar |
| F57-D1 | [Carry: Chromatic / Code Connect / deprecated](1750000145000-f57-chromatic-code-connect-deprecated.md) | listo para ejecutar |
| F57-E1 | [Tools DX: checks scaffolds + smoke scaffolds](1750000146000-f57-tools-dx-scaffold-smoke.md) | listo para ejecutar |
| F57-F1 | [Documentación ronda](1750000147000-f57-documentation.md) | listo para ejecutar |

## Orden sugerido

1. **A1 → A2** (coverage) — desbloquea confianza en reportes CI.
2. **B1 → B2 → E1** (scaffolds) — en paralelo con A si hay manos.
3. **C1** — desbloquea `arq:fe:build:smoke` React sin `--excludeTaskDependencies`.
4. **D1** — solo si hay token Chromatic / Figma; si no, defer F58 con motivo.
5. **F1** — cierre docs + hub.

## Criterios de aceptación (ronda)

- [ ] Informe coverage **por proyecto** coincide con lo ejecutado (sin paredes 0% fantasma ni merges mentirosos).
- [ ] Script/gate que valida inventario coverage (soft en CI → strict cuando A2 lo diga).
- [ ] CLI scaffolds + README; un dominio nuevo se genera con tags `runtime:*` correctos.
- [ ] `pnpm arq:fe:build:smoke` React sin exclude de deps **o** defer C1 documentado.
- [ ] D1 hecho o defer F58.
- [ ] Hub `docs/plans` + biblia alineados.

## Fuera de alcance (F58+)

- Umbrales globales % coverage en todo el monorepo (solo inventario + verdad por proyecto aquí).
- Sustituir Jest por Vitest.
- Generadores Nx oficiales en lugar de scripts locales (evaluar, no migrar).
