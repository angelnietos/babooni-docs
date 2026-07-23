<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F74-A2 — NestJS Contract Integration</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F74" src="https://img.shields.io/badge/round-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

Los handlers y controladores NestJS consumen schemas Zod de `@base/shared` mediante Pipes
reutilizables, eliminando la brecha de validación entre frontend y backend.

## Entregables

1. **`ZodValidationPipe` genérico** en `libs/base/shared/src/lib/core/`:
   `pipe.transform(value, schema)` → `value` o `BadRequestException` con `issues`.
2. Decoradores NestJS (`@ZodValidate()`) por dominio o Pipe global por endpoint.
3. Migración de ≥3 controladores NestJS (ej: `clients`, `users`, `auth`) para usar
   `createClientSchema` / `updateClientSchema` importados directamente desde `@base/shared`.
4. Tests unitarios del Pipe genérico.

## Reglas

- El Pipe no importa `class-validator` (Zod es la única fuente de verdad).
- Errores Zod se mapean a `ValidationError` NestJS con `issues` legibles.
- `clients.controller.ts` pasa a `CreateClientDto` definido como `z.infer<typeof createClientSchema>`.
