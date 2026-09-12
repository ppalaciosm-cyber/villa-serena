# Guía de contribución

## Ramas
- `main` — solo versiones estables (fin de sprint). Protegida.
- `develop` — integración continua del sprint. Protegida, requiere PR.
- `feature/<modulo>-<descripcion>` — ej. `feature/reservas-crear-reserva`
- `fix/<descripcion>` — correcciones
- `docs/<descripcion>` — documentación

Flujo: `feature/*` → PR a `develop` → revisión de al menos 1 compañero → merge. Al cerrar sprint: `develop` → `main`.

## Commits (Conventional Commits)
```
<tipo>(<alcance>): <descripción corta en español>

feat(reservas): crear endpoint POST /reservas
fix(web): corregir filtro de habitaciones por piso
docs(scrum): agregar sprint 2
chore: configurar husky
```
Tipos: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`.

## Pull Requests
- Título con el mismo formato que los commits.
- Enlazar la historia de usuario / issue.
- CI en verde (lint, typecheck, tests).
- Sin `console.log`, sin credenciales, sin código comentado.

## Código
- TypeScript estricto en todos los paquetes.
- Nombres en español para el dominio (`Reserva`, `Habitacion`), en inglés para lo técnico (`useQuery`, `AuthGuard`).
- Formatear antes de commitear (lo hace lint-staged automáticamente).
