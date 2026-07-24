# Cliente Ideauto — qué implica la migración

Hub: [../README.md](../README.md)

## Resumen para negocio

Vamos a **modernizar** Recalls dentro de la plataforma Babooni/Arquetipos sin apagar el sistema de un día para otro. El servicio sigue disponible; las piezas se van sustituyendo por detrás (strangler).

## Garantías

- Continuidad: legacy activo hasta validar cada slice.  
- Rollback rápido vía enrutado si un slice falla.  
- DGT y documentos críticos se validan en paralelo antes del corte.  
- No se mezcla con otros productos (Verifactu/SaaS): Recalls sigue siendo **vuestro** producto Ideauto.

## Qué pediremos

- Disponibilidad para validar pantallas/PDF en staging (sobre todo presupuestos y cartas).  
- Ventana acordada para el cutover final (M6).  
- Congelar features grandes en legacy durante la migración (hotfixes sí).

## Lectura

- [why-migrate.md](../why-migrate.md)  
- [risks-and-rollback.md](../risks-and-rollback.md)  
- [milestones.md](../milestones.md)
