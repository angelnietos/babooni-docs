# PM — delivery Ideauto Recalls

Hub: [../README.md](../README.md)

## Tablero

Usa [milestones.md](../milestones.md) como SoT de estado.  
Tickets de ronda: [migration](../../migration/).

## Dependencias

| Dependencia | Impacto |
|-------------|---------|
| **F83** (MSSQL / Prisma) | Bloquea M0–M2 si no hay adapter |
| Proxy / feature flags | Necesario desde M1 |
| Inventario permisos authguard | Necesario antes de M1 |
| Sign-off PDF negocio | Gate M3 |
| Parallel-run DGT | Gate M4 |

## Rituales sugeridos

- Semanal: avance M*, riesgos, PRs abiertos  
- Por milestone: demo staging + checklist gate  
- No abrir M6 sin M4+M5 verdes  

## Lectura

1. [migration-strategy.md](../migration-strategy.md)  
2. [risks-and-rollback.md](../risks-and-rollback.md)  
3. [stakeholders/developers.md](./developers.md)
