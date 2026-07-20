# Sonar — análisis local y alineación con CI

## Herramientas

| Herramienta | Uso |
|-------------|-----|
| **SonarLint** (IDE) | Reglas en tiempo real sobre `apps/`, `libs/`, `tools/scripts/` |
| **SonarScanner** (opcional) | `sonar-scanner` con `sonar-project.properties` en la raíz |

## Alcance

`sonar-project.properties` excluye:

- `**/generated/**` (Prisma, etc.)
- `**/*.spec.ts`, `**/*.stories.ts`
- `node_modules`, `dist`, `coverage`

## Scripts de smoke (F13)

Los scripts en `tools/scripts/*verifactu*.mjs` y `preflight-dev-infra.mjs` siguen estas convenciones para evitar hallazgos Sonar:

| Regla | Mitigación |
|-------|------------|
| **S7785** top-level await | Usar `await` a nivel módulo en `.mjs` (Node ESM) |
| **S4036** PATH en comandos OS | `docker` vía `execFile('docker', [...])`; health checks en contenedor con `docker compose exec` |
| **S4030** colecciones sin uso | No acumular en arrays si solo se hace `console.log` |
| **S7763** re-export | `export { x } from './mod'` en barriles TypeScript |

Supresión documentada en `sonar-project.properties` solo para **S7785** en `tools/scripts/**` si el servidor Sonar no reconoce top-level await en ESM.

## Verificación manual

```bash
node tools/scripts/preflight-dev-infra.mjs
node tools/scripts/smoke-verifactu-worker.mjs
node --check tools/scripts/run-verifactu-smoke.mjs
```

## CI (opcional F14)

Job dedicado con servicio Redis + `pnpm saas:worker:smoke` — no mezclar con `verify` principal (requiere Docker service).

## Relación con ESLint

Module boundaries y UI ownership siguen en ESLint (`pnpm check:ui-ownership`, `pnpm hygiene`). Sonar complementa calidad de scripts y duplicación en libs.
