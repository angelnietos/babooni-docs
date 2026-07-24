# Producto — decisiones Recalls

Hub: [../README.md](../README.md)

## Tu trabajo en esta migración

- Validar que el **alcance de negocio** (campañas, oleadas, DGT, facturación) se preserva.  
- Priorizar slices (auth → core campañas → DGT → PDF).  
- Firmar paridad visual de PDFs/cartas (M3) y cutover (M6).  
- **No** pedir features nuevas en legacy salvo hotfixes críticos.

## Decisiones ya tomadas

| Decisión | Valor |
|----------|-------|
| Colocación | Cliente Ideauto, no SaaS |
| Estrategia | Strangler (ADR 0013) |
| FE | Next (como legacy) |
| BE | Nest + Prisma |

## Lectura

1. [why-migrate.md](../why-migrate.md)  
2. [milestones.md](../milestones.md)  
3. [risks-and-rollback.md](../risks-and-rollback.md)
