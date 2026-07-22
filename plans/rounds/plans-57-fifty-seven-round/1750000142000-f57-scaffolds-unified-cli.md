# F57-B1 — Scaffolds unificados (CLI + README)

## Estado

completado

## Objetivo

Convertir `tools/scaffolds/` de scripts sueltos en un **kit DX** usable:
entrada única, ayuda clara, dry-run consistente y documentación en biblia.

## Estado actual

| Script | Rol |
|--------|-----|
| `gen-domain.mjs` | Generador “todo en uno” (scopes base/arquetipos/josanz/saas) |
| `new-domain.mjs` | Stub hex backend `@base` |
| `scaffold-arquetipos-domain.mjs` | 4 capas Angular/React plantilla |
| `scaffold-arquetipos-app.mjs` | App shell plantilla |
| `scaffold-josanz-domain.mjs` | Dominio producto Josanz |
| `scaffold-cliente-product.mjs` | Producto cliente nuevo |
| `scaffold-base-data-access.mjs` | Data-access base puntual |

Problemas: solapamiento gen-domain vs scaffold-*, sin README, sin `pnpm scaffold …`,
`--help` irregular, post-pasos (paths, tags) fáciles de olvidar.

## Entregables

1. `tools/scaffolds/README.md` — mapa, cuándo usar cada uno, ejemplos, verificación.
2. CLI `tools/scaffolds/cli.mjs` (o `index.mjs`):
   ```bash
   node tools/scaffolds/cli.mjs <command> …
   # commands: domain | app | cliente | help
   ```
   - `domain` → delega a `gen-domain` / `scaffold-arquetipos-domain` / `scaffold-josanz-domain` según `--scope`.
   - `app` → `scaffold-arquetipos-app`.
   - `cliente` → `scaffold-cliente-product`.
3. Scripts `package.json`:
   - `scaffold` → CLI help
   - `scaffold:domain`, `scaffold:app`, `scaffold:cliente` (wrappers).
4. Todos los scripts: `--help` exit 0 + mensaje Usage; `--dry-run` no escribe disco.
5. Guía [scaffold-arquetipos.md](../../../guides/scaffold-arquetipos.md) +
   [guides/README](../../../guides/README.md) + [tools-layout](../../../runbooks/tools-layout.md)
   apuntan al CLI.

## Criterios de aceptación

- [x] `pnpm scaffold` muestra help con subcomandos.
- [x] Dry-run de dominio arquetipos + josanz no crea archivos.
- [x] README scaffolds enlazado desde `tools/README.md`.
- [x] Sin romper scripts existentes (paths estables; CLI es capa fina).

## Verificación

```bash
pnpm scaffold
pnpm scaffold:domain -- --scope arquetipos --domain demo57 --dry-run
node tools/scaffolds/scaffold-josanz-domain.mjs --help
```
