# Versionado de API HTTP

## Objetivo

Mantener la compatibilidad de consumidores y comunicar cambios de API de forma
estándar y automatizable.

## Estrategia

- **Versionado por ruta** solo en breaking mayor:
  - `GET /v1/clients` → `GET /v2/clients` implica cambio incompatible.
  - Dentro de la misma versión, solo se permiten cambios no breaking.
- **Deprecación por header** en rutas existentes:
  - `Sunset: <date>` — fecha de eliminación planeada.
  - `Deprecation: true` — indicador de que la ruta está deprecada.

## Reglas

### Rutas deprecadas

```http
GET /v1/clients HTTP/1.1
Sunset: Tue, 31 Dec 2026 23:59:59 GMT
Deprecation: true
```

- El header `Sunset` debe ser un HTTP-date válido.
- La fecha debe ser al menos **2 releases menores** después del release que introduce la deprecación.
- El cuerpo de la respuesta puede incluir un campo informativo:

```json
{
  "deprecated": true,
  "sunset": "2026-12-31T23:59:59.000Z",
  "replacement": "/v2/clients"
}
```

### Breaking mayor

- Introducir nueva ruta versionada (`/v2/...`).
- Mantener la ruta vieja (`/v1/...`) como deprecated con `Sunset`.
- Eliminar `/v1/...` en un release mayor posterior.

### Breaking menor

- No cambiar rutas ni métodos.
- Solo añadir campos opcionales o nuevas rutas.

## Aplicación en NestJS

```typescript
import { deprecate } from '@nestjs/common';

@Get('v1/clients')
@deprecate('Usar /v2/clients. Sunset: 2026-12-31')
@Header('Sunset', 'Tue, 31 Dec 2026 23:59:59 GMT')
@Header('Deprecation', 'true')
findV1() {
  return this.clientsService.findAll();
}
```

## Estado actual

- Workspace bajo Nx `23.1.0` con Node `22.x`.
- No hay rutas versionadas activas en este release; la política está definida para el próximo breaking mayor.
- CI ejecuta `pnpm check:deprecated` y `pnpm check:node-nx` para prevenir drift.

## Cumplimiento

- CI verifica que toda ruta deprecated tenga `Sunset` y `Deprecation` mediante inspección de código o decoradores.
- Documentación OpenAPI (`@ApiHeader`) debe reflejar los headers de deprecación.

## Referencias

- [Política de deprecación](./deprecation-policy.md)
