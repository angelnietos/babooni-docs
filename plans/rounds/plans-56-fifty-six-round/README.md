# Ronda F56 — Apps arquetipos verdes, Jest BE, MockServer FE-only

## Estado

listo para ejecutar

> **Arranque:** tras F55 completada.
> F55 dejó Lit oleada 2, a11y, coverage BE + parity strict, Jest preset.
> F56 valida que las **plantillas arquetipos compilan y se ven bien**,
> profundiza **testing en backend**, y entrega un **MockServer global** para
> que front trabaje **sin API / DB**.

## Dependencias externas

- F55 cerrada.
- Token Chromatic / Figma Code Connect siguen opcionales (carry F55-B1/D1).
- No exigir Keycloak/Postgres para el modo mock FE.

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F56-A1 | [Build smoke apps arquetipos (compilan)](1750000130000-f56-arquetipos-apps-build-smoke.md) | Nuevo |
| F56-A2 | [Visual / serve “se ven bien” (PW soft)](1750000131000-f56-arquetipos-apps-visual-qa.md) | Carry F55-B2 |
| F56-B1 | [Jest + coverage backend apps/libs](1750000132000-f56-backend-jest-coverage-extend.md) | Carry F55-C3 |
| F56-C1 | [MockServer global (fixtures + CLI)](1750000133000-f56-mockserver-global.md) | Nuevo |
| F56-C2 | [FE arquetipos → proxy mock (sin API)](1750000134000-f56-fe-mockserver-wiring.md) | Nuevo |
| F56-D1 | [Carry: Chromatic / Code Connect / remove deprecated](1750000135000-f56-carry-chromatic-code-connect-deprecated.md) | Carry F55 |
| F56-E1 | [Documentación y cierre F56](1750000136000-f56-documentation.md) | Cierre |

## Objetivo

1. Gate CI/local: apps plantilla **build** verdes (Angular/React single ± multi).
2. Señal visual: serve + Playwright soft (login mock o mockserver) — “se ve bien”.
3. Extender Jest/coverage a backends Nest (apps + `@josanz/backend` / SaaS) sin bajar umbrales C1.
4. **MockServer** workspace: un proceso HTTP con fixtures `/api/*` alineados a contratos.
5. Fronters: `pnpm arq:fe:*:mock` (o equivalente) sin levantar api-gateway/Postgres.
6. Carry F55 residual documentado (Chromatic/Code Connect/remove) o defer F57.

## Orden de ejecución

1. **A1** build smoke (rápido, desbloquea confianza).
2. **C1** MockServer + fixtures canario (auth/clients/users).
3. **C2** cablear proxies Angular/React al mock.
4. **A2** visual QA usando mock (sin API real).
5. **B1** Jest BE en paralelo a C*.
6. **D1** según tokens; no bloquea.
7. **E1** cierre.

## Riesgos

- Build apps Angular/React pesado en CI → affected o job nightly + canario single.
- Mock desalineado con DTOs reales → fixtures desde `@base/shared` / OpenAPI snapshot.
- Auth/Keycloak: en modo mock, login demo debe ser stub (sin IdP) o MSW en browser.
- No mezclar umbrales `test:cov:check` (dominios F55-C1) con coverage workspace BE.

## Criterios de aceptación (ronda)

- [ ] ≥2 apps arquetipos FE (`angular-single`, `react-single`) **build** verdes en CI o script documentado.
- [ ] Visual soft reproducible localmente (serve + checklist o PW) **o** defer justificado.
- [ ] Backend: ≥1 oleada Jest/coverage BE apps/libs documentada + `check:jest-preset` intacto.
- [ ] MockServer arranca con un comando; FE single apunta al mock sin API.
- [ ] Runbook mock en `docs/runbooks/` + local-development actualizado.
- [ ] Carry Chromatic/Code Connect/remove resuelto o defer F57.
- [ ] Hub F56 archivo al cerrar; checks convención verdes.

## Relación con F55

| F55 deja | F56 profundiza |
|----------|----------------|
| Parity smoke documentado | Build + visual QA de apps |
| Jest preset 100% libs | Coverage/scripts BE apps Nest |
| Proxy → gateway :4000 | Proxy → MockServer (FE-only) |
| Chromatic/Code Connect defer | Reintento o defer F57 |
| Remove deprecated pendiente | Carry D1 |
