<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F59-A1 — Estrategia validación isomórfica (ADR + kit)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

trasladado → F60-E1 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Definir **cómo** compartir reglas de validación entre backend Nest y frontends
(Angular / React / futuros) sin duplicar `required` / `email` / longitudes /
patrones en cada capa.

Preferencia: **isomórfico** (mismo módulo en `libs/*/shared`). Si alguna regla
no puede ser isomórfica (p. ej. async uniqueness vía DB), dejar **puertos**
genéricos reutilizables por dominio/app (sync rules en shared; async en BE +
mensaje FE).

## Situación actual

| Capa | Qué hay |
|------|---------|
| `@base/shared` DTOs | `class-validator` decorators (`CreateClientDto`, …) |
| Nest apps | `ValidationPipe({ transform, whitelist })` |
| Angular features | `Validators.required/email/pattern` ad-hoc (login, Josanz create, …) |
| React | Poco / inconsistente; re-exporta DTOs sin validar en form |

## Entregables

1. **ADR corto** (`docs/adr/adr-00XX-isomorphic-validation.md`) con decisión:
   - **Opción recomendada a evaluar:** schemas/predicates puros en
     `@base/shared` (o `@base/shared/validation`) → (a) decorators
     class-validator que delegan, **o** (b) Zod/Standard Schema como SoT y
     adapters BE/FE. Documentar trade-offs vs status quo.
   - Regla: sync → shared; async (unique email, …) → port BE + código error
     estable para FE.
2. Estructura de carpetas propuesta:
   `libs/base/shared/src/lib/validation/` (primitives + por dominio).
3. Guía en `docs/guides/` (enlace desde extend-kernel-domain / backend-domain).
4. Decisión explícita: **no** introducir segunda librería en paralelo sin ADR.

## Criterios de aceptación

- [ ] ADR merged + enlace desde `docs/adr/README.md`.
- [ ] Kit vacío o stub tipado exportado (`@base/shared` / subpath) listo para A2.
- [ ] Tabla “cuándo isomórfico vs genérico por dominio/app”.
- [ ] Sin romper apps existentes (sin migración masiva en A1).

## Verificación

```bash
pnpm nx typecheck base-shared   # o proyecto Nx de @base/shared
pnpm check:lib-layout:strict
```
