# F46-A3 — Corregir mismatch de tipos React en `ArqTabs`

## Estado

completado

## Objetivo

Resolver el error de typecheck en `base-react-native-ui` causado por
mismatch entre `@types/react` 19.x (proyecto) y 18.x (react-native interno).

## Tareas

1. Abrir `libs/base/frontend/mobile/rn/ui/src/lib/ArqTabs.tsx`.
2. Cambiar la línea:
   ```tsx
   <View style={styles.panel}>{children(active)}</View>
   ```
   por:
   ```tsx
   <View style={styles.panel}>{children(active) as any}</View>
   ```
3. Ejecutar `pnpm nx typecheck base-react-native-ui` para confirmar que pasa.

## Criterios de aceptación

- [ ] `base-react-native-ui:typecheck` pasa sin errores.
- [ ] El cast queda documentado en código como excepción localizada.
