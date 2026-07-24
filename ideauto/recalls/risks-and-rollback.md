<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="48" alt="Arquetipos" />
</p>

<h1 align="center">Riesgos y rollback</h1>

<p align="center">
  <a href="./README.md"><img alt="Recalls" src="https://img.shields.io/badge/hub-Recalls-0f766e?style=flat-square" /></a>
</p>

---

## Riesgos

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| SOAP DGT divergente | Legal / operativo | Parallel-run; adapter único; contract tests XML |
| Prisma / MSSQL quirks | Blocker M0–M2 | F83 checklist; smoke staging |
| Paridad PDF / cartas | Reclamaciones | Golden files + sign-off negocio |
| Features ocultas authguard-core | Huecos auth | Inventario permisos antes de M1 |
| Doble mantenimiento largo | Coste | Ventana strangler acotada; no features en legacy |
| Colocación errónea (SaaS) | Arquitectura | Gates docs + `layer:clientes` |

---

## Rollback

| Nivel | Acción |
|-------|--------|
| Feature flag | Proxy vuelve a Express para ese path |
| Release | Revert PR del dominio |
| Catástrofe | PITR MSSQL (ops) |

**Invariante:** durante strangler, no migraciones destructivas en tablas compartidas sin dual-write o freeze.

---

## Continuidad de negocio

- Legacy permanece hasta M6.  
- DGT: legacy puede seguir siendo fuente de verdad hasta parallel-run verde.  
- Comunicar a marcas solo cortes planificados (M6).

---

## Enlaces

- [migration-strategy.md](./migration-strategy.md)  
- [stakeholders/client.md](./stakeholders/client.md)
