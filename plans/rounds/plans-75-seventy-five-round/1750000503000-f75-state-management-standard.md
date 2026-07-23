<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-B1 — State Management Standard Across Frameworks</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrado

## Objetivo

Estandarizar las estrategias y librerías de gestión de estado para todos los frameworks (Angular, Ionic, React, Next.js y React Native), separando explícitamente **Estado de Servidor** y **Estado de Cliente UI**.

## Regla de Oro de Gestión de Estado

```text
Server State (API Async) ───► TanStack Query (isomórfico para React, Angular, Vue, etc.)
Client State (UI Local)  ───► Angular: Signals + NgRx Signal Store
                              React / Next / RN: Zustand
```

## Configuración Específica por Framework

### 1. Angular & Ionic Angular

#### Server State (`data-access/queries`, `data-access/mutations`)
- **Librería**: `@tanstack/angular-query-experimental`
- **Patrón**: Encapsulado en servicios inyectables o funciones de consulta.
```ts
@Injectable({ providedIn: 'root' })
export class UsersQueries {
  private api = inject(UsersApiClient);

  getUsersQuery() {
    return injectQuery(() => ({
      queryKey: ['users'],
      queryFn: () => this.api.getUsers(),
    }));
  }
}
```

#### Client State (`data-access/store`)
- **Librería**: Angular Signals + `@ngrx/signals` (Signal Store).
- **Responsabilidad**: Modales, filtros de tabla, pestañas activas, selecciones.

---

### 2. React, Next.js & React Native / Ionic React

#### Server State (`data-access/queries`, `data-access/mutations`)
- **Librería**: `@tanstack/react-query`
- **Patrón**: Custom Hooks declarativos expuestos desde `data-access`.
```ts
export function useUsersQuery() {
  return useQuery({
    queryKey: ['users'],
    queryFn: fetchUsersApi,
  });
}
```

#### Client State (`data-access/store`)
- **Librería**: `zustand`
- **Patrón**: Stores atomizados por subdominio o pantalla.
```ts
export const useUsersStore = create<UsersState>((set) => ({
  selectedUserId: null,
  setSelectedUserId: (id) => set({ selectedUserId: id }),
}));
```

---

## Comparativa de Adopción Arquitectónica

| Framework | Server State | Client UI State | Form State | Validaciones |
|-----------|--------------|-----------------|------------|--------------|
| **Angular** | TanStack Query | Signals / Signal Store | Reactive Forms | Zod (`@base/shared/validation`) |
| **Ionic Angular** | TanStack Query | Signals / Signal Store | Reactive Forms | Zod (`@base/shared/validation`) |
| **React** | TanStack Query | Zustand | React Hook Form | Zod (`@base/shared/validation`) |
| **Next.js** | TanStack Query | Zustand | React Hook Form | Zod (`@base/shared/validation`) |
| **React Native** | TanStack Query | Zustand | React Hook Form | Zod (`@base/shared/validation`) |

## Ventajas del Estándar
1. **Reutilización**: Los tipos, contratos y llamadas a la API (`api/`) son 100% idénticos.
2. **Cero Ficción**: Se aprovecha la herramienta más nativa e idiomática de cada ecosistema.
3. **Productividad**: Los desarrolladores y agentes de IA usan exactamente el mismo flujo de datos en todos los dominios.
