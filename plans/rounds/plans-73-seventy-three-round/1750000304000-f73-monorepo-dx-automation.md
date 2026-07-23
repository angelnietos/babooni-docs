<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F73-C1 — Monorepo DX & Isomorphic Governance Automation</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F73" src="https://img.shields.io/badge/round-F73-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

---

## Objetivo

Automatizar la gobernanza de contratos isomórficos, la generación de scaffolds agnósticos y la verificación en CI de la arquitectura portable entre frontend y backend.

---

## Motivación

Para prevenir desviaciones futuras y asegurar que los nuevos dominios sigan los contratos isomórficos desde el primer día, es necesario actualizar el conjunto de scripts de scaffolding (`tools/scaffolds/`) y verificaciones linter (`tools/checks/`).

---

## Entregables

1. **Linter de Contratos Isomórficos (`check-domain-conventions.mjs`):**
   - Verificar que todos los DTOs expuestos por los paquetes `api` provengan de `@base/shared-contracts` sin redefiniciones locales.
2. **Generador de Dominios Isomórficos (`gen-domain.mjs`):**
   - Actualizar para crear automáticamente las entradas en `@base/shared-contracts`, el ViewModel puro en `logic/` y los adaptadores correspondientes según el stack.
3. **Comprobación de Paridad en CI (`check-arquetipos-parity.mjs`):**
   - Asegurar que los arquetipos de producto hereden limpiamente los contratos isomórficos de la base sin romper tipado.

---

## Criterios de aceptación

- [ ] `node tools/checks/check-domain-conventions.mjs` valida la ausencia de DTOs duplicados.
- [ ] Ejecutar `node tools/scaffolds/gen-domain.mjs` genera automáticamente la infraestructura isomórfica (Contracts + ViewModel Vanilla + Adaptadores + 4 subcarpetas no vacías).
- [ ] Los pipelines de CI bloquean cualquier PR que introduzca tipos DTO desacoplados.
