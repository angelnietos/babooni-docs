<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-C1 — Generators & Scaffolding Tooling</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrado

## Objetivo

Actualizar y ampliar la suite de generadores automáticos (`tools/scaffolds/`) para crear estructuras completas de 8 capas o elementos específicos de capa de manera consistente en todos los frameworks.

## CLI de Generadores F75

```bash
# Generador Maestro de Dominio Completo
node tools/scaffolds/gen-domain.mjs <domain-name> --framework=<angular|react|next|ionic|react-native>

# Subgeneradores de Capa Atomizados
node tools/scaffolds/gen-api.mjs <domain-name>
node tools/scaffolds/gen-data-access.mjs <domain-name> --type=<query|mutation|store>
node tools/scaffolds/gen-feature.mjs <domain-name> <feature-name>
node tools/scaffolds/gen-shell.mjs <domain-name>
node tools/scaffolds/gen-ui.mjs <domain-name> <component-name>
node tools/scaffolds/gen-store.mjs <domain-name> --framework=<angular|react>
```

## Estructura Generada por `generate-domain users --framework=next`

```text
libs/base/frontend/next/users/
├── domain/
│   ├── src/
│   │   ├── models/user.model.ts
│   │   └── index.ts
│   └── project.json
├── api/
│   ├── src/
│   │   ├── clients/users-client.ts
│   │   ├── endpoints/users-endpoints.ts
│   │   ├── dto/user.dto.ts
│   │   ├── mappers/user.mapper.ts
│   │   └── index.ts
│   └── project.json
├── data-access/
│   ├── src/
│   │   ├── queries/use-users-query.ts
│   │   ├── mutations/use-user-mutations.ts
│   │   ├── store/use-users-store.ts
│   │   └── index.ts
│   └── project.json
├── features/
│   ├── src/
│   │   ├── pages/UsersPage.tsx
│   │   ├── components/UsersList.tsx
│   │   ├── logic/useUsersController.ts
│   │   └── index.ts
│   └── project.json
├── shell/
│   ├── src/
│   │   ├── layout/UsersShellLayout.tsx
│   │   └── index.ts
│   └── project.json
├── ui/
│   ├── src/
│   │   ├── components/UserBadge.tsx
│   │   └── index.ts
│   └── project.json
├── shared/
│   ├── src/
│   │   ├── constants/user-constants.ts
│   │   └── index.ts
│   └── project.json
└── testing/
    ├── src/
    │   ├── mocks/users-mock.ts
    │   └── index.ts
    └── project.json
```

## Plantillas de Subgeneradores

1. **`gen-api`**: Scaffoldea cliente HTTP e interfaces DTO integradas con `@base/shared/validation`.
2. **`gen-data-access`**: Scaffoldea queries/mutations de TanStack Query y store de Zustand/Signals.
3. **`gen-feature`**: Scaffoldea controller/service ViewModel y páginas conectadas a data-access.
4. **`gen-shell`**: Scaffoldea layout principal del dominio y provider wrappers.
5. **`gen-ui`**: Scaffoldea componentes de presentación puros en el directorio `ui/`.
