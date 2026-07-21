# F45-B1 — Mejora de harness de tests

## Estado

completado

## Objetivo

Reducir la fragilidad en specs de backend removiendo dependencias a
`AnyConstructor` y mejorando helpers compartidos del test harness.

## Entradas

- F44-B1 (specs remanentes) — baseline de errores resueltos.
- `libs/base/backend/src/test/` — helpers existentes.

## Tareas

1. **Auditar uso de `AnyConstructor`**:
   - Buscar `AnyConstructor` en `*.spec.ts` y `*.int-spec.ts`.
   - Categorizar: ¿es realmente necesario o se puede tipar?
2. **Mejorar `controller.harness.ts`**:
   - Reemplazar `AnyConstructor` por genéricos tipados:
     ```ts
     export function createTestingModule<T extends Type<any>>(controllers: T[]) {
       return TestBed.configureTestingModule({
         imports: controllers.map((c) => c),
       });
     }
     ```
3. **Agregar helper para Prisma**:
   - Crear `prisma-test.harness.ts` que abstraiga la creación de
     `PrismaMultiService` con mocks, eliminando la necesidad de casteos en
     specs de integración.
4. **Actualizar specs frágiles**:
   - Reemplazar casteos `as any` / `as unknown as Type` por helpers tipados.
5. **Verificar**:
   - `pnpm nx typecheck base-backend` pasa.
   - `pnpm nx test base-backend` pasa.

## Restricción

No cambiar comportamiento de producción; solo refactor de tests.

## Criterios de aceptación

- [ ] No hay `AnyConstructor` en specs nuevos.
- [ ] No hay casteos `as any` en specs de backend.
- [ ] `pnpm nx test base-backend` pasa.

## Dependencias

- Ninguna.
