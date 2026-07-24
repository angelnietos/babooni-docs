<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="48" alt="Arquetipos" />
</p>

<h1 align="center">Por qué migrar Recalls_v2</h1>

<p align="center">
  <b>Negocio · riesgo · coste de no hacer nada</b>
</p>

<p align="center">
  <a href="./README.md"><img alt="Recalls" src="https://img.shields.io/badge/hub-Recalls-0f766e?style=flat-square" /></a>
</p>

---

## El problema en una frase

Recalls_v2 **cumple la función**, pero está construido fuera del motor Arquetipos: dos repos, Express/JS sin capas, Sequelize atado a MSSQL, auth casera y poca reutilización. Cada mes de deuda suma riesgo (seguridad, DGT, PDFs, onboarding) y bloquea mejoras que el monorepo ya tiene.

---

## Qué es Recalls hoy (legacy)

Producto Ideauto de **campañas de recall vehicular**:

- Altas de campaña y gestión de VINs  
- Oleadas postales y telemáticas  
- Integración **DGT** (SOAP)  
- Presupuestos, facturas, certificados  
- Informes (renting, VIN, …) y panel admin  

Stack legacy: **Express 5 + Sequelize + MSSQL** (server) y **Next 16 + React 19 + Redux** (client). Sin Nx, sin `@base/*`, sin contrato de capas.

---

## Por qué es necesario dejar de hacerlo “así”

### 1. Seguridad y cumplimiento

La API documenta flujos de usuarios con superficie pública amplia. La identidad depende de JWT propio + paquete externo `@ideauto/authguard-core`. El monorepo estandariza IdP (Keycloak) y guards Nest. Migrar auth **no es cosmética**: reduce exposición y unifica auditoría.

### 2. Dominio imposible de aislar

~166 services planos en el backend. Campaña, DGT, PDF y ficheros se cruzan. Un bug en oleadas puede tocar facturación. En Arquetipos cada dominio tiene módulo Nest + capas FE: cambia menos superficie por PR.

### 3. Base de datos atada

Sequelize + MSSQL only. El monorepo invierte en Prisma multi-provider (**F83**) para poder operar MSSQL (y otros) con el mismo dominio. Sin migrar, Ideauto no aprovecha esa inversión.

### 4. Coste de equipo y DX

Dos lockfiles, sin `nx affected`, sin module boundaries, tests escasos vs tamaño del código. Onboarding = arqueología. El monorepo da CI, typecheck, coverage y convenciones compartidas con Josanz/plataforma.

### 5. Estrategia de producto

Mientras Recalls viva fuera:

- no reutiliza auth, audit, clients, documents del kernel;  
- cada mejora de plataforma **no llega** al producto;  
- se mantienen **dos culturas** de ingeniería.

---

## Qué gana el cliente (Ideauto)

| Hoy | Después |
|-----|---------|
| Parches reactivos de seguridad | Auth y gates alineados a plataforma |
| Features lentas por acoplamiento | Dominios entregables por slice |
| Riesgo en cutovers | Strangler + rollback por flag |
| Dependencia de conocimiento tribal | Docs + convenciones monorepo |
| Stack divergente | Mismo motor que el resto de productos Babooni |

---

## Qué **no** es esta migración

- No es un rediseño de negocio DGT (el contrato externo se preserva).  
- No es meter Recalls en **SaaS / Verifactu** (`@saas/*`).  
- No es big-bang “apagamos el lunes”.  
- No es copiar carpetas legacy al monorepo: se **reconstruye** sobre `@ideauto/*` + `@base/*`.

---

## Coste de no migrar

1. Deuda de seguridad y overrides npm crece.  
2. Cada feature nueva se escribe en el anti-patrón.  
3. Imposible compartir kernel (auth, audit, PDF helpers).  
4. DGT y PDF siguen en un runtime frágil (jobs in-process, poca cobertura).  
5. El equipo no escala: solo quien conoce el monolito puede tocar recalls.

---

## Decisión

Migrar con **strangler** a:

```
apps/clientes/ideauto/recalls
libs/clientes/ideauto  →  @ideauto/*
```

Detalle: [migration-strategy.md](./migration-strategy.md) · [ADR 0013](../../adr/adr-0013-recalls-strangler-migration.md).

---

## Enlaces

- [legacy-vs-target.md](./legacy-vs-target.md)  
- [stakeholders/product.md](./stakeholders/product.md)  
- Ejecución: [migration/](../migration/)
