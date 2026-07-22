# F57-E1 — Tools DX: smoke de scaffolds + checks

## Estado

listo para ejecutar

## Objetivo

Añadir red de seguridad para que scaffolds y tools no se rompan en silencio:
smoke dry-run en CI (o script local gate) y alineación con `tools/` reorganizado.

## Entregables

1. `tools/scaffolds/smoke-scaffolds.mjs` (o bajo `tools/smoke/`):
   - Ejecuta `--help` / `--dry-run` de CLI + gen-domain + scaffold-arquetipos-domain.
   - Exit ≠0 si crash o Usage missing.
2. `pnpm scaffolds:smoke` en package.json; opcional soft en CI `frontend`/`quality`.
3. Revisar que ningún doc operativo cite `tools/scripts/` (excepto legacy-paths / planes históricos).
4. Opcional: `check:scaffolds-readme` — falla si falta README en `tools/scaffolds/`.

## Criterios de aceptación

- [ ] `pnpm scaffolds:smoke` verde en local.
- [ ] Soft en CI o documentado como gate local obligatorio en PR de scaffolds.
- [ ] tools-layout menciona scaffolds CLI tras B1.

## Verificación

```bash
pnpm scaffolds:smoke
rg -n "tools/scripts/" docs --glob '!docs/plans/**' --glob '!**/legacy-paths.md' || true
```
