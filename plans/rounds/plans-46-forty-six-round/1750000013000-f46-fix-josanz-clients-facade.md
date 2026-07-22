<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F46-A4 — Corregir facade `josanz-clients-data-access`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Alinear `JosanzClientsFacade.deleteClient()` con la API pública de
`ClientsService` base, que expone `remove()` en lugar de `delete()`.

## Tareas

1. Abrir `libs/clientes/josanz/angular/clients/data-access/src/lib/josanz-clients.facade.ts`.
2. Reemplazar:
   ```typescript
   deleteClient(id: string): Observable<boolean> {
     return from(this.clientsApi.delete(id)).pipe(
   ```
   por:
   ```typescript
   deleteClient(id: string): Observable<boolean> {
     return from(this.clientsApi.remove(id)).pipe(
   ```
3. Ejecutar `pnpm nx typecheck josanz-clients-data-access` para confirmar que pasa.

## Criterios de aceptación

- [ ] `josanz-clients-data-access:typecheck` pasa sin errores.
- [ ] No se modifican call sites fuera de la facade.
