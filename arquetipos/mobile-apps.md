<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Arquetipos mobile — Ionic y React Native (Expo)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: el producto prioriza app móvil (o híbrida) y necesitas un modelo.

---

## 1. React Native + Expo (`react-native-single` / `multi`)

- **Default** para nativo en el motor.
- Expo arranca Metro; `--web` para demos rápidas (puertos 8091/8092).
- Auth demo `AuthApp` + features clients/users.
- **Metro:** pin React 18 (evitar React 19 hoisted del monorepo).

Guía: [../guides/add-mobile-domain.md](../guides/add-mobile-domain.md).

**Cuándo elegirlo:** el cliente quiere store apps / UX nativa (ej. Josanz “mobile > web”).

---

## 2. Ionic (`ionic-single` / `multi`)

- Híbrido Capacitor-friendly; puertos 4300/4301.
- Útil si el equipo viene de web components / Angular móvil.

---

## 3. Qué no hacer

- No uses RN CLI “porque sí” — Expo cubre el 90%; CLI solo con módulos nativos duros.
- No asumas que el SPA responsive reemplaza estos arquetipos.
- No olvides typecheck / Metro clear tras cambiar deps React.

---

## Enlaces

- [catalog.md](./catalog.md)
- [../architecture/framework-decision-guide.md](../architecture/framework-decision-guide.md)
- [../frontend/ui-strategy.md](../frontend/ui-strategy.md) — RN UI ≠ Lit CE
