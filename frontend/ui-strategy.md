# Estrategia UI — Lit native-ui + wrappers por framework

Cuándo usarla: debates “¿componentes nativos compartidos o solo Angular Material
/ shadcn por stack?”, Storybook, o al añadir un primitivo nuevo.

---

## 1. Problema que resolvemos

Sin estrategia común:

- Button distinto en Angular, React, Next, RN → N implementaciones.
- La IA inventa markup por feature.
- Cambiar design tokens implica chase en 4 repos mentales.

Con estrategia:

```
Una SoT de primitivos (Lit CE) → wrappers idiomáticos → marca de producto
```

---

## 2. Capas UI en este monorepo

| Capa | Paquete | Tecnología | Rol |
|------|---------|------------|-----|
| **A. Primitivos crosscutting** | `@base/native-ui` | Lit Custom Elements | Button, input, card, modal… framework-agnostic |
| **B. Adapter framework** | `@base/angular-ui`, `@base/react-*`, `@base/next-ui`, `@base/react-native-ui` | Wrapper Angular/React/RN | API ergonómica + integración change detection / props |
| **C. Marca / plantilla** | `@josanz/angular-ui`, `@arquetipos/*-ui`, `@saas/shared-ui` | Extiende B | Branding, defaults, templates |

Regla wrap vs re-export en C: [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md).

---

## 3. Por qué Lit / Custom Elements

- Corren en **cualquier** host que entienda DOM (Angular, React, Next, incluso estático).
- Encapsulación Shadow DOM / estilos acotados.
- Encaja con la visión “base común para todos los proyectos” sin forzar un solo
  framework FE en la empresa.
- RN **no** usa Lit en native: tiene su propio `@base/react-native-ui` (primitivos RN);
  la alineación es de **design tokens / API conceptual**, no del mismo CE.

---

## 4. Flujo al añadir un primitivo (ej. `Badge`)

1. ¿Es genérico? → implementar/estabilizar en `@base/native-ui` (+ tests/Storybook si aplica).
2. Exponer wrapper en `@base/angular-ui` / React si el stack lo consume vía API tipada.
3. Actualizar [ui-component-catalog.yaml](./ui-component-catalog.yaml).
4. Producto: re-export o wrapper de marca.
5. `pnpm check:ui-ownership`.

No empieces el primitivo dentro de una feature Josanz.

---

## 5. Storybook

- Documenta variantes del **paquete que posee** el componente.
- Plantillas Arquetipos pueden tener stories de wrappers premium.
- Objetivo de infraestructura: Storybook integrado en el motor (gobernanza visual),
  no un Storybook por feature suelta.

---

## 6. Relación con “stack oficial Next de la empresa”

Aunque otros equipos usen Next/Tailwind/RTK por defecto:

- Este monorepo **ya** tiene Angular (ERP), React, Next, RN.
- Los **primitivos Lit** permiten no acoplar el design system a Tailwind de un solo stack.
- Next puede envolver CEs o usar `@base/next-ui`; no bloquea el motor.

La elección Next vs React SPA para un *producto nuevo* está en
[framework-decision-guide](../architecture/framework-decision-guide.md) — independiente
de dónde viven los átomos visuales.

---

## 7. Anti-patrones

| Mal | Bien |
|-----|------|
| Copiar Button Angular a React a mano | Extender native-ui + wrapper |
| Feature con CSS suelto “provisional” eterno | Token / UI package |
| Wrapper Josanz vacío “por si acaso” | Re-export |
| Un solo Storybook de producto mezclando capas | Ownership por paquete |

---

## Enlaces

- [design-system.md](./design-system.md)
- [how-it-works.md](./how-it-works.md)
- Código: `libs/base/frontend/crosscutting/native-ui/`
