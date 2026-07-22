<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Arquetipos frontend web — Angular, React, Next, MF</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: localizar la plantilla FE correcta tras decidir stack.

---

## 1. Angular SPA (`angular-single` / `angular-multi`)

- **Modelo preferido** para ERP/backoffice en este motor (Josanz).
- Lazy `loadChildren` → `@arquetipos/angular-{domain}-shell`.
- Single vs multi: branding / switcher tenant.

**Copia la idea de:** `app.routes.ts`, providers auth, layout shell.

---

## 2. React SPA (`react-single` / `react-multi`)

- Misma arquitectura 4 capas que Angular (paridad canario clients).
- Útil cuando el equipo es React y **no** hace falta Next.

Paridad: [../frontend/dual-stack-clients-parity.md](../frontend/dual-stack-clients-parity.md)
(canario clients). Marco multi-framework + tooling:
[F54-B4](../plans/rounds/plans-54-fifty-four-round/1750000104500-f54-arquetipos-cross-framework-parity.md).

---

## 3. Next.js (`next-single` / `next-multi`)

- Opt-in cuando hay SSR/SEO/App Router de verdad.
- Thin libs Next → base; no mezclar `@josanz` en plantilla.

Guía: [../guides/add-next-domain.md](../guides/add-next-domain.md).  
Decisión vs React SPA: [../architecture/framework-decision-guide.md](../architecture/framework-decision-guide.md).

---

## 4. Module Federation

- Host Angular (`mf-host-angular`) + remotes React (`mf-remote-*`).
- Solo si el producto exige remotes independientes.

DX: [../guides/module-federation-dev.md](../guides/module-federation-dev.md).

---

## 5. Qué mirar en cada app (checklist)

- [ ] `app.routes` / router solo compone shells
- [ ] Auth alineada con realm del API
- [ ] Imports `@arquetipos/...` o path a base — no deep relatives rotos
- [ ] E2E smoke asociado (`*-e2e`) como referencia de prueba mínima

---

## Enlaces

- [../frontend/how-it-works.md](../frontend/how-it-works.md)
- [../frontend/arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md)
- [catalog.md](./catalog.md)
