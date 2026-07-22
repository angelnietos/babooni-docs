# Cómo escribir documentación en este monorepo

Guía de estilo para la **biblia operativa** (`docs/`). Los planes en `docs/plans/`
siguen su propia plantilla de ronda; no los reescribas por estética.

## Jerarquía de verdad

| Prioridad | Ubicación |
|-----------|-----------|
| 1 — Biblia | `docs/README.md`, `architecture/`, `guides/`, `backend/`, `frontend/`, `runbooks/`, `adr/` |
| 2 — Agentes / CI | `AGENTS.md`, `tools/` (ver `tools/README.md`) |
| 3 — Planes | `docs/plans/` (trabajo en curso o histórico) |

Si un plan contradice la biblia, **gana la biblia**.

## Idioma

- Biblia operativa (hub, architecture, guides, frontend, runbooks ampliados): **español**.
- ADRs: formato [MADR](https://adr.github.io/madr/); pueden quedar en inglés si ya están accepted.
- Términos técnicos (`shell`, `harness`, `outbox`) se dejan en inglés.

## Plantilla de guía (`docs/guides/*.md`)

```markdown
# Título — verbo + objeto

Cuándo usarla: una frase.

## Idea / contexto
…

## Pasos
1. …
2. …

## Verificación
\`\`\`bash
pnpm nx typecheck <project>
\`\`\`

## Enlaces
- …
```

## Reglas de formato

1. **H1 único** = título del documento.
2. Primeras 2–3 líneas = *cuándo usarlo* (descubrimiento rápido).
3. Mapas y decisiones → **tablas**; procedimientos → **listas numeradas**.
4. Mermaid solo para flujos (capas, apps↔libs, pirámide de tests). Sin colores custom.
5. Links **relativos** (`./`, `../`). Nunca paths absolutos de máquina (`/Users/…`).
6. Cada guía nueva: bloque final **Verificación** con comandos `pnpm` / `nx`.
7. Al añadir un doc: actualizar el índice del área (`guides/README.md`, etc.) **y** el hub [`README.md`](./README.md) si es P0/P1.

## ADRs

- Archivo: `docs/adr/adr-NNNN-kebab-title.md` (número secuencial).
- Actualizar la tabla en [`adr/README.md`](./adr/README.md).
- Status: `accepted` | `proposed` | `superseded` (+ enlace al ADR nuevo).

## Checklist PR (docs)

- [ ] Índice de área actualizado
- [ ] Links relativos resuelven (sin `../../../` fuera del repo)
- [ ] Sin paths absolutos de máquina
- [ ] Hub enlaza docs P0/P1 nuevos
- [ ] Si cambia una convención de código, hay sección Verificación alineada con scripts CI

## Enlaces

- [docs/README.md](./README.md) — hub
- [guides/README.md](./guides/README.md)
- [runbooks/tools-layout.md](./runbooks/tools-layout.md) — mapa `tools/`
- [guides/npm-publish-and-versioning.md](./guides/npm-publish-and-versioning.md) — publicar libs canario + workflow manual
- [plans/README.md](./plans/README.md) — rondas activas / siguientes
- [CONTRIBUTING.md](../CONTRIBUTING.md) — contribuir código
- [`tools/README.md`](../tools/README.md) — utilidades (código)