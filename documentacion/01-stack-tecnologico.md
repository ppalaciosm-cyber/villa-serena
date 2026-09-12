# Stack tecnológico

Documento de referencia para el equipo. Cada decisión relevante tiene (o tendrá) su ADR en `docs/arquitectura/adr/`.

## Resumen

| Capa              | Tecnología                                   | Por qué                                                                 |
|-------------------|----------------------------------------------|-------------------------------------------------------------------------|
| Frontend web      | React 19 + Vite + TypeScript + Tailwind v4   | Ya es el stack del prototipo (Figma Make). Se reutilizan las pantallas. |
| Routing web       | React Router v7                              | Rutas por rol (`/admin`, `/recepcion`, `/huesped`…), guards por rol.    |
| Estado servidor   | TanStack Query                               | Cache, refetch y estados de carga de la API sin boilerplate.            |
| Estado cliente    | Zustand                                      | Sesión, módulo activo, UI global. Ligero y fácil de aprender.           |
| Formularios       | React Hook Form + Zod                        | Validación compartida con el backend vía `packages/shared`.             |
| Componentes UI    | shadcn/ui (Radix + Tailwind)                 | Componentes accesibles que se copian al repo; encaja con Tailwind v4.   |
| Backend           | NestJS (Node 22 + TypeScript)                | Arquitectura modular: cada módulo del hotel es un módulo de Nest. Mismo lenguaje que el front → tipos compartidos. |
| ORM               | Prisma                                       | Esquema declarativo, migraciones versionadas, tipado automático.        |
| Base de datos     | PostgreSQL 16                                | Relacional (reservas, pagos, inventario lo requieren). Docker en local. |
| Autenticación     | JWT (access + refresh) con Passport          | Roles: admin, recepcion, limpieza, mantenimiento, roomservice, huesped. |
| Tiempo real       | Socket.IO (Nest Gateway)                     | Estado de habitaciones, pedidos de room service, solicitudes de huésped.|
| Documentación API | Swagger (OpenAPI) generado por Nest          | Contrato entre front, back y móvil.                                     |
| Validación API    | class-validator + Zod (shared)               | DTOs validados en el borde de la API.                                   |
| Testing           | Vitest (web/shared) · Jest + Supertest (api) | Tests unitarios y e2e de endpoints.                                     |
| Lint / Formato    | ESLint + Prettier (oxfmt opcional)           | Reglas compartidas en `packages/config`.                                |
| Git hooks         | Husky + lint-staged + commitlint             | Conventional Commits obligatorios.                                      |
| CI                | GitHub Actions                               | Lint + typecheck + tests en cada PR.                                    |
| Contenedores      | Docker Compose                               | PostgreSQL (y opcionalmente la API) en local.                           |
| Monorepo          | pnpm workspaces (+ Turborepo opcional)       | Un solo `pnpm install`, paquetes compartidos.                           |
| Móvil (futuro)    | React Native + Expo + Expo Router            | Reutiliza `packages/shared` y el cliente API. Solo vista huésped.       |
| Despliegue        | Vercel (web) · Railway/Render (api + db)     | Gratis o casi gratis para proyecto universitario.                       |

## Módulos del sistema → módulos de la API

| Módulo (rol)      | Pantallas (prototipo)                                              | Módulos NestJS                                   |
|-------------------|--------------------------------------------------------------------|--------------------------------------------------|
| Admin             | Panel habitaciones, Tarifas, Reportes, Personal, Inventario        | `habitaciones`, `tarifas`, `reportes`, `usuarios`, `inventario` |
| Recepción         | Día, Reservas, Disponibilidad, Huéspedes, Habitaciones, Solicitudes, Comprobante | `reservas`, `huespedes`, `habitaciones`, `pagos`, `solicitudes` |
| Limpieza          | Inicio, Mapa, Solicitudes, Incidencias, Objetos olvidados, Historial | `limpieza`, `incidencias`, `solicitudes`        |
| Mantenimiento     | Panel, Bandeja incidencias, Órdenes de trabajo, Preventivo, Activos | `mantenimiento`, `incidencias`                  |
| Room Service      | Pedidos, Menú, Historial, Cargos                                    | `roomservice`, `pagos`                           |
| Huésped           | Inicio, Reservar, Check-in web, Mi habitación, Servicios, Cuenta    | `reservas`, `solicitudes`, `roomservice`, `pagos`, `notificaciones` |

Transversales: `auth`, `usuarios`, `notificaciones` (WebSocket), `reportes`.

## Autenticación y roles
- Personal (admin, recepción, limpieza, mantenimiento, room service) inicia sesión con usuario/contraseña.
- Huésped inicia sesión con código de reserva + documento, o correo/contraseña si crea cuenta.
- Access token JWT de corta duración (15 min) + refresh token (7 días).
- Guards de Nest por rol (`@Roles('admin', 'recepcion')`).
- El front oculta rutas por rol, pero la API es la que realmente autoriza.

## Flujo de datos
```
Web / Móvil ──HTTP (REST/JSON)──▶ NestJS ──Prisma──▶ PostgreSQL
      ▲                              │
      └────── WebSocket (eventos) ───┘
```

## Alternativas descartadas
- **Express puro**: sin estructura, cada persona haría las cosas distintas. Nest impone orden con 6 personas.
- **Spring Boot / .NET / Laravel**: sólidos, pero implicarían dos lenguajes y perder los tipos compartidos con el front y la app móvil.
- **MongoDB**: el dominio es fuertemente relacional (reservas ↔ habitaciones ↔ pagos ↔ huéspedes).
- **Flutter** para móvil: obligaría a reescribir tipos, validaciones y cliente API en Dart.
