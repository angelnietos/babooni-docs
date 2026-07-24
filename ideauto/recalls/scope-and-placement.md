<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="48" alt="Arquetipos" />
</p>

<h1 align="center">Alcance y colocación</h1>

<p align="center">
  <a href="./README.md"><img alt="Recalls" src="https://img.shields.io/badge/hub-Recalls-0f766e?style=flat-square" /></a>
</p>

---

## Destino canónico

```
apps/clientes/ideauto/recalls/
├── backend/     # Nx: ideauto-recalls-api (M0 stub → Nest)
└── frontend/    # Nx: ideauto-recalls-web (M0 stub → Next; ADR 0008)

libs/clientes/ideauto/
├── shared/              # @ideauto/shared          ← M0 stub
├── backend/             # @ideauto/backend         ← M0 stub
├── angular-ui/          # @ideauto/angular-ui      ← M0 stub (FE producto = Next)
└── angular/platform/    # @ideauto/platform-{api,data-access,shell} ← M0 stub
    # + dominios Next/FE (M1+): {campaigns,dgt,waves,…}/{api,data-access,shell,features}
```

| Concepto | Valor |
|----------|-------|
| Cliente slug | `ideauto` |
| Producto / app | `recalls` |
| npm | `@ideauto/*` |
| Apps Nx | `ideauto-recalls-api`, `ideauto-recalls-web` |
| Nx tag | `layer:clientes` (+ `runtime:*`) |
| Importa | `@base/*`, `@ideauto/*` |
| **Prohibido** | `@arquetipos/*`, `@saas/*`, `@josanz/*` |
| Estado M0 | **Scaffold hecho** — stubs sin dominio de negocio |

---

## Por qué `clientes/ideauto` y no SaaS

| | Cliente Ideauto | SaaS |
|--|-----------------|------|
| Quién | Producto de **cliente** Ideauto | Productos genéricos vendibles (Verifactu…) |
| Path | `apps/clientes/ideauto/…` | `apps/productos-saas/…` |
| npm | `@ideauto/*` | `@saas/*` |
| Marca | Recalls / DGT Ideauto | CRM fiscal / ledger |

Meter Recalls en `@saas/*` mezclaría marcas y rompería boundaries. Ver [nuevo-cliente-checklist](../../clientes/nuevo-cliente-checklist.md).

---

## En scope / fuera de scope

### En scope (migración)

- Auth, campañas, oleadas, VINs, presupuestos/facturas/PDF, DGT, addresses, reports, admin, workers  
- Strangler con proxy/flags  
- Docs y gates CI  

### Fuera de scope

- Cambiar reglas de negocio DGT  
- Features nuevas solo en legacy (salvo hotfix)  
- Apagar legacy antes de paridad (M6)  
- Código Josanz / Verifactu  

---

## Relación con planes F84

Los tickets viven en:

https://github.com/Babooni-Technologies/arquetipos/tree/main/docs/plans/rounds/plans-84-eighty-four-round

Esta carpeta `docs/ideauto/recalls/` es la **narrativa de producto**. F84 es el **checklist de ronda**.

---

## Enlaces

- [domain-map.md](./domain-map.md)  
- [milestones.md](./milestones.md)  
- Hub Ideauto: [../README.md](../README.md)
