# F60-E1 — Carry F59: validación isomórfica FE↔BE

## Estado

listo para ejecutar

## Objetivo

Ejecutar el trabajo **no hecho** de F59 A1–A4 + D1: fuente de verdad de reglas
en shared, piloto clients, adapters Angular/React, scaffolds/gate, envelope
Problem+JSON.

## Origen (F59)

| F59 | Título |
|-----|--------|
| [A1](../plans-59-fifty-nine-round/1750000160000-f59-isomorphic-validation-strategy.md) | Estrategia ADR + kit |
| [A2](../plans-59-fifty-nine-round/1750000161000-f59-validation-pilot-clients.md) | Piloto clients |
| [A3](../plans-59-fifty-nine-round/1750000162000-f59-validation-fe-adapters.md) | Adapters + error envelope FE |
| [A4](../plans-59-fifty-nine-round/1750000163000-f59-validation-scaffolds-gate.md) | Scaffolds + gate drift |
| [D1](../plans-59-fifty-nine-round/1750000168000-f59-validation-error-codes.md) | Problem+JSON / códigos |

## Entregables

1. ADR isomórfico + stub `@base/shared` validation.
2. Piloto Create/Update client: mismas sync rules Nest + un form Angular + un
   form React.
3. Adapters `toAngularValidators` / resolver React + mapeo errores API.
4. Códigos error estables documentados.
5. Scaffold/check soft de drift (strict opcional).

## Criterios de aceptación

- [ ] Checklist de aceptación de cada plan F59 origen marcado o superado aquí.
- [ ] Sin migración masiva de todos los dominios (solo piloto + plantilla).

## Verificación

Ver secciones Verificación de A1–A4/D1 originales.

## Depende de

Nada (paralelo a A/C visual). Idealmente no bloquea D1 visual.

## Si no hay presupuesto

Defer F61 con lista explícita — no silencio.
