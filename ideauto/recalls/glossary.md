# Glosario — Ideauto Recalls

| Término | Significado |
|---------|-------------|
| **Recalls_v2** | Sistema legacy (ideauto-server + ideauto-client) |
| **Ideauto** | Cliente del monorepo (`layer:clientes`, `@ideauto/*`) |
| **Recalls** | Producto de campañas de recall bajo Ideauto |
| **Strangler** | Migración por slices con legacy vivo detrás de proxy |
| **Slice / milestone** | Entrega vertical (API + FE + tests) con gate |
| **`@base/*`** | Kernel compartido del monorepo |
| **`@ideauto/*`** | Paquetes del cliente Ideauto |
| **`@saas/*`** | Productos SaaS (Verifactu…) — **no** usar para Recalls |
| **DGT** | Dirección General de Tráfico — integración SOAP |
| **Oleada / wave** | Envío postal o telemático de una campaña |
| **VIN** | Identificador de vehículo |
| **F83** | Ronda de portabilidad DB (MSSQL vía Prisma) |
| **F84** | Ronda de plan de migración Recalls |
| **Composition root** | App que cablea módulos (`apps/…`) |
| **4 capas FE** | `api → data-access → features ← ui`; `shell` lazy-load |

Hub: [README.md](./README.md)
