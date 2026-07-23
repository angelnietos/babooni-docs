<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F74-A1 — Full Isomorphic Coverage (Zod por dominio)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F74" src="https://img.shields.io/badge/round-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

Cobertura isomórfica del 100%: cada dominio con DTO en `@base/shared` tiene un archivo
`validation/<domain>.ts` (predicados síncronos) y `validation/<domain>.schema.ts` (Zod) en
`libs/base/shared/src/lib/validation/`.

## Entregables

1. `users.ts` + `users.schema.ts` — `UserFieldErrors`, `validateCreateUser`, `validateUpdateUser`.
2. `auth.ts` + `auth.schema.ts` — `LoginInput`, `RegisterInput`, predicates.
3. `roles.ts` + `roles.schema.ts` — `CreateRoleInput`, `UpdateRoleInput`.
4. `billing.ts` + `billing.schema.ts` — `InvoiceDto`, `CreateInvoiceDto`.
5. `inventory.ts` + `inventory.schema.ts` — `ProductDto`, `StockMoveDto`.
6. `projects.ts` + `projects.schema.ts` — `ProjectDto`, `CreateProjectDto`.
7. `settings.ts` + `settings.schema.ts` — `SettingDto`, `UpsertSettingDto`.
8. `tenants.ts` + `tenants.schema.ts` — `TenantDto`, `CreateTenantDto`.
9. Actualizar `validation/index.ts` para exportar todo.

## Reglas

- Cada schema Zod usa `.safeParse()` con helper `zodValidate*(input)` que devuelve `Record<string,string>`.
- Predicados síncronos devuelven `Partial<Record<key, string>>` (compat NestJS Pipe + FE forms).
- Los archivos `.schema.ts` NO exportan `ZodIssue` tipado para mantener compat Zod v4.
