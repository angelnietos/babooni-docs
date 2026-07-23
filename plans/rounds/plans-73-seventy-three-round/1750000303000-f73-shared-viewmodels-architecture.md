<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F73-B1 — Portable Vanilla TS ViewModels & Cross-Framework State Adapters</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F73" src="https://img.shields.io/badge/round-F73-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

---

## Objetivo

Establecer el patrón de **ViewModels en Vanilla TypeScript puro (agnósticos de framework)** en la carpeta `logic/` de cada feature, permitiendo que la lógica de presentación y el estado de la UI sean totalmente portables y reutilizables entre Angular, React, Next.js, Ionic y React Native.

---

## Motivación

En la Ronda F72 estandarizamos la carpeta `logic/` en todos los features. Sin embargo, para alcanzar el máximo grado de agustismo e isomorfismo, la lógica contenida dentro de `logic/` debe escribirse en TypeScript puro y exponerse mediante pequeñas capas adaptadoras idóneas para cada framework.

---

## Arquitectura de ViewModels Agnósticos

```text
features/src/logic/
├── controllers/
│   └── ClientsController.ts      (Clase/ViewModel Vanilla TS puro, sin imports de UI)
├── adapters/
│   ├── useClientsController.ts   (Custom Hook adaptador para React / Next / React Native)
│   └── clients-panel.service.ts  (Injectable o Signal Store adaptador para Angular / Ionic)
└── index.ts                      (Exportación unificada)
```

### Reglas de los ViewModels Puros:
1. Sin importaciones de React, Angular o paquetes vinculados al ciclo de vida del framework.
2. Manejo de estado mediante observables agnósticos (RxJS puros, Zustand vanilla o pub/sub simple).
3. Conexión directa con `@base/shared-contracts` para consumo de datos y validación.

---

## Entregables

1. Refactorización del controlador del dominio `clients` como caso piloto de ViewModel 100% portable.
2. Implementación de los adaptadores ligeros para React/Next/RN (`useClientsController`) y Angular/Ionic (`ClientsPanelService`).
3. Guía de arquitectura para desarrollo de ViewModels Isomórficos.

---

## Criterios de aceptación

- [ ] `ClientsController.ts` no contiene importaciones de ningún framework de UI.
- [ ] Mismo `ClientsController` utilizado en una prueba de integración simultánea entre React y Angular.
- [ ] Cobertura de pruebas unitarias al 100% en la capa `logic/` sin necesidad de renderizar componentes visuales.
