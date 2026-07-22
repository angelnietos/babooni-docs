<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F00 — Node.js ↔ Nx compatibility policy</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Definir y mantener una política explícita de compatibilidad entre Node.js y Nx,
incluyendo versiones soportadas, cómo comprobarlo y qué hacer cuando el language
server de Nx en VS Code falle por incompatibilidad.

## Contexto

El workspace usa Nx `23.1.0`. Esa versión no soporta Node.js `24.x`: los plugin
workers de Nx se cierran antes de establecer conexión, generando errores como:

```
Plugin worker ... exited with code 0 before the connection was established.
```

El CLI (`npx nx`) puede seguir funcionando, pero el language server de Nx en
VS Code (Angular Console) queda inservible.

## Tareas

1. **Definir matriz de versiones soportadas**:
   - Nx `23.x` → Node `20.x` / `22.x`
   - Nx `24.x` → Node `22.x` / `24.x`
2. **Añadir comprobación en CI**:
   - `node --version` debe estar dentro del rango permitido por el `engines` de
     Nx o por una lista explícita en el plan.
3. **Documentar procedimiento de recuperación**:
   - Recargar VS Code tras cambios de versión.
   - Si el language server sigue fallando, borrar cache de Nx:
     `pnpm nx reset`
4. **Prevenir drift futuro**:
   - Añadir un script `check:node-nx` que compare `node --version` contra la
     matriz y falle si no es compatible.

## Restricción

No mezclar versiones de Nx y Node fuera de la matriz definida sin antes abrir
un plan específico de actualización.

## Criterios de aceptación

- [ ] Matriz de versiones documentada en este plan.
- [ ] CI falla si Node.js está fuera de rango.
- [ ] Equipo conoce el procedimiento de recuperación del language server.

## Dependencias

- F44 (deprecation hygiene) — para cerrar el plan de redondeo actual.
