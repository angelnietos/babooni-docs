# Nx daemon (F36-P0-B)

## Política por SO

Nx sigue siendo la herramienta de orquestación del monorepo. La única variable
es el daemon en segundo plano:

| SO | Recomendación | Por qué |
|----|---------------|---------|
| Linux / macOS | **Daemon activo** por defecto | Estable; el cacheo del grafo compensa (~3s warm vs ~10s sin daemon). |
| Windows | **`NX_DAEMON=false` por defecto** | Con ~366 proyectos y plugins con fan-out, el daemon se cuelga con frecuencia; el costo de operación supera el beneficio. |

CI corre en Linux y ya mantiene `NX_DAEMON=false` solo como workaround histórico;
cuando se estabilice, se puede revertir.

## Síntoma

- En Windows, `nx show projects` / `nx test` fríos pueden tardar **>60s** o
  quedar pegados; plugin workers con timeouts de 10s bajo presión de procesos.
- En Linux/macOS el daemon es estable y más rápido cuando está caliente.
- Cuando el daemon está saludable, `nx show projects` warm tarda ~3s; sin daemon,
  ~10s.

## Evidencia (2026-07-18, Windows, ~366 proyectos, limpieza F36-P0-A aplicada)

| Modo | Comando | Tiempo aprox. |
|------|---------|---------------|
| `NX_DAEMON=false` frío | `pnpm exec nx show projects` | ~27s |
| `NX_DAEMON=true` tras `nx reset` | mismo | ~71s |
| `NX_DAEMON=false` caliente | mismo | ~10s |
| `NX_DAEMON=true` caliente | mismo | ~3s |

## Causa probable

**Grafos grandes + plugin workers**, no un proyecto malo:

1. Tamaño del grafo — cientos de proyectos Nx (web, Next, Ionic, RN, SaaS).
2. Fan-out de plugins — `@nx/eslint`, `@nx/jest`, `@nx/vite`, `@nx/playwright`,
   más `tools/scripts/nx-typecheck-plugin.mjs`. El registro/handshake frío es
   costoso en Windows.
3. Presión de procesos — muchos `nx run *:serve` concurrentes → plugin workers
   que no entregan handshake a tiempo (ver [pnpm-layout.md](./pnpm-layout.md)).
4. Ruido de artefactos anidados — históricamente empeoraba el indexing;
   mitigado por F36-P0-A (`check:node-modules`).

## Configuración recomendada

**Linux / macOS:**

```bash
# opcional: el daemon se activa solo
pnpm exec nx show projects
```

Si querés forzarlo:

```bash
npx nx daemon --start
```

**Windows:**

```powershell
$env:NX_DAEMON = 'false'
pnpm exec nx show projects   # frío ~27s, caliente ~10s
```

O de forma permanente en `.env` (ver `.env.example`):

```
NX_DAEMON=false
```

`pnpm postinstall` advierte en Windows si falta la variable.

## Comandos útiles

```bash
# Estado del daemon (solo informativo en Windows)
pnpm nx:daemon:status

# Si se colgó: detener servidores, luego
pnpm nx daemon --stop
pnpm nx reset

# Typecheck / lint / test afectados (sin daemon funciona igual)
pnpm verify:affected
pnpm typecheck:affected

# Jest directo cuando `nx test` se traba
pnpm exec jest -c libs/base/backend/jest.config.ts --passWithNoTests
```

## Plugin worker timeouts (Windows)

Si incluso con daemon desactivado los workers timed out:

```powershell
$env:NX_ISOLATE_PLUGINS = 'false'
# o
$env:NX_PLUGIN_NO_TIMEOUTS = 'true'
pnpm exec nx show projects
```

## Re-habilitar el daemon en Windows

Solo si un smoke job con `NX_DAEMON=true` queda verde en frío y caliente en CI
y Windows local. Hasta entonces, tratar los cuelgues como esperables y usar
`NX_DAEMON=false`.

## Plugin excludes

`nx.json` ya excluye árboles Expo/Ionic/Next de plugins **jest** y **vite** donde
esos stacks no los usan. No silenciar **eslint** para esas apps sin agregar
targets `lint` explícitos (ver F36-P0-C).
