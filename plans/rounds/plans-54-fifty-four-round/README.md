# Ronda F54 — Madurez native-ui, migración masiva, visual CI e imports

## Estado

listo para ejecutar

> **Arranque:** tras **F53-F1** (o en paralelo solo docs si F53 aún cierra A1/B*).
> F53 sigue siendo la ronda de **política + Storybook + wrappers base**.
> F54 profundiza: migración de apps, oleadas de import externo, a11y/visual,
> tokens compartidos, deprecación gradual de primitivos framework-only, CI SB.

## Dependencias externas

- F53 (native-first, SB serve/native, wrappers parity, soft gate, inventory A3).
- Carry típico desde F53: imports masivos, coverage strict si no cerró D1,
  publish oleada UI, gate native-first → strict.

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F54-A1 | [Migración masiva features → native wrappers](1750000100000-f54-migrate-features-to-native-wrappers.md) | Carry F53-C1 |
| F54-A2 | [Oleadas import UI externos → Lit](1750000101000-f54-external-ui-import-waves.md) | Carry F53-A3 |
| F54-A3 | [Deprecar primitivos framework-only en base](1750000102000-f54-deprecate-framework-only-primitives.md) | Carry F53-A1 |
| F54-B1 | [CI build-storybook native + visual / Chromatic](1750000103000-f54-storybook-ci-and-visual-regression.md) | Carry F53-B2 |
| F54-B2 | [Paquete `@base/ui-tokens` + consumo RN/web](1750000104000-f54-shared-ui-tokens-package.md) | Carry F53-E1 |
| F54-B3 | [A11y + teclado/focus en catálogo Lit](1750000105000-f54-native-ui-a11y-pass.md) | Calidad SoT |
| F54-B4 | [Paridad arquetipos: arch + native-ui + funcional](1750000104500-f54-arquetipos-cross-framework-parity.md) | DX / calidad plantillas |
| F54-C1 | [Gate `ui-native-first` strict + allowlist ratchet](1750000106000-f54-ui-native-first-strict.md) | Carry F53-C2 |
| F54-C2 | [Ampliar oleada npm publishable UI](1750000107000-f54-publishable-ui-wave.md) | Carry F52/F53 |
| F54-D1 | [Figma / Code Connect ↔ native-ui](1750000108000-f54-figma-code-connect-native-ui.md) | Design↔code |
| F54-D2 | [Coverage BE strict (si F53-D1 defer)](1750000109000-f54-coverage-ci-strict-carry.md) | Carry F52/F53 |
| F54-E1 | [Documentación y cierre F54](1750000110000-f54-documentation.md) | Cierre |

## Objetivo

1. Que **features** de arquetipos/clientes/SaaS consuman wrappers native por
   defecto (no solo demos canario).
2. Portar oleadas concretas de UI externa a Lit (no inventario suelto).
3. Empezar **deprecation** real de Button/Input Angular|React paralelos en base
   (JSDoc `@deprecated`, codemods opcionales) sin big-bang delete.
4. Storybook native en CI + señal visual (Chromatic u homólogo).
5. Tokens compartidos web↔RN; pase a11y del catálogo Lit.
6. Gate native-first **strict**; ampliar publishable UI; opcional Code Connect.
7. **Paridad arquetipos** (B4): misma **arquitectura** 4 capas/thin, misma **UI**
   via native-ui, mismos **flujos** — medido con `check:arquetipos-parity`.

## Orden de ejecución

1. **A1** migración features (tras F53-A2 wrappers completos).
2. **A2** imports (prioridad del inventario F53-A3).
3. **B2 + B3 + B4** tokens/a11y/paridad arquetipos en paralelo a A* (B4 puede
   ir tras B1 si reutiliza visual CI).
4. **B1** CI SB cuando Storybook native F53 esté verde.
5. **A3** deprecation solo cuando A1 haya reducido usos.
6. **C1** strict tras ventana soft F53-C2.
7. **C2 / D1 / D2** según capacidad.
8. **E1** cierre.

## Riesgos

- Migración masiva sin wrappers → fricción; bloquear A1 si F53-A2 incompleto.
- Deprecar demasiado pronto rompe Josanz (~400 componentes marca — **no**
  deprecar `@josanz/angular-ui` branding; solo paralelos en `@base/*-ui`).
- Chromatic/coste CI — soft o nightly primero.
- Code Connect requiere Figma access / MCP — defer si no hay token.

## Criterios de aceptación (ronda)

- [ ] ≥ N features migradas (definir N en Resultado A1; mínimo arquetipos
      clients+users Angular o React).
- [ ] ≥ 1 oleada A2 con componentes en native-ui + stories.
- [ ] Lista deprecation publicada + al menos 1 primitivo base marcado
      `@deprecated` con alternativa Native*.
- [ ] `build-storybook base-native-ui` en CI (o nightly documentado).
- [ ] Tokens package o extract documentado + consumo RN/web.
- [ ] Checker/manifest paridad arquetipos (B4) soft en CI o defer documentado.
- [ ] Gate strict **o** defer con evidencia.
- [ ] Hub: F54 archivo o activa; F53 en archivo tras su F1.
- [ ] Checks convención verdes.

## Relación con F53

| F53 entrega | F54 profundiza |
|-------------|----------------|
| Política freeze | Deprecation mecánica (A3) |
| Wrappers 14 CEs | Migración features (A1) |
| Inventario externos | Oleadas de código (A2) |
| SB serve + native SB | CI + visual (B1) |
| Soft gate | Strict (C1) |
| Matriz tokens RN | Paquete tokens (B2) |
| Canario clients Angular↔React | Paridad multi-stack + tooling (B4) |
