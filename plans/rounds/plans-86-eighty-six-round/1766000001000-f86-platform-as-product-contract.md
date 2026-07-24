# F86-A — Platform-as-product contract

Estado: en progreso · 2026-07-24

## Objetivo

Declarar este monorepo como la **base oficial de empresa**: qué capas son
obligatorias, qué es opt-in, y qué contratos toda app debe cumplir.

## Acciones

- [x] Publicar [platform-as-product.md](../../../architecture/platform-as-product.md)
- [ ] ADR corto (0015) si el contrato introduce decisiones irreversibles
- [ ] Enlazar desde biblia hub + getting-started + nuevo-cliente checklist
- [ ] Matriz “must / should / may” por tipo de app (cliente, SaaS, plantilla, MS)
- [x] Incluir en contrato: config server FE/BE ([F86-F](1766000006000-f86-platform-config-server.md)) y plano Microsoft/Azure ([F86-G](1766000007000-f86-microsoft-azure-data-plane.md))

## Criterios

- [ ] Un onboarding lee un solo doc y sabe qué reutilizar de `@base/*`
- [ ] Ninguna app nueva inventa Prisma/adapters/auth fuera del kernel
- [ ] Clientes Microsoft saben que SQL Server/Azure es path soportado (no experimental)