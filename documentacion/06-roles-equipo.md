# Roles del equipo — Villa Serena

Equipo: Carlos, Alex, Hugo, Josue, Kim, Pablo (Scrum Master).
Principio: cada persona es dueña de un **módulo vertical** (front + back) y de una **responsabilidad transversal**. Los módulos rotan cada sprint para que todos toquen todo.

## Roles Scrum
| Rol | Persona | Qué hace |
|-----|---------|----------|
| Scrum Master | Pablo | Facilita ceremonias, mantiene el tablero, quita bloqueos, revisa PRs, habla con el catedrático |
| Product Owner (proxy) | Kim | Dueño del prototipo/diseño; prioriza el backlog y valida que lo desarrollado coincida con las vistas |
| Equipo de desarrollo | Todos | Front + back de su módulo asignado |

## Sprint 1 — Fundamentos (1–2 semanas)
Objetivo: que cualquiera pueda clonar, levantar web + api + base de datos y hacer login.

| Persona | Responsabilidad transversal | Entregables Sprint 1 |
|---------|-----------------------------|----------------------|
| **Pablo** | Infraestructura + Auth | `package.json` raíz, `pnpm-workspace.yaml`, `docker-compose.yml` (Postgres), `packages/config`, CI en GitHub Actions, husky/commitlint. Módulo `auth` + `usuarios` en la API (JWT, roles, guards). Tablero en GitHub Projects. |
| **Kim** | Líder de frontend | Base de `apps/web`: migrar el prototipo, `router/`, `layouts/`, `components/ui` (shadcn), `lib/api/client.ts`, `store/`, pantalla de Login conectada al auth. |
| **Carlos** | Líder de backend / Base de datos | Scaffold de NestJS, `prisma/schema.prisma` completo (todas las entidades a partir de `types.ts`), `seed.ts` con datos demo, `database/` module, Swagger. Diagrama ER en `documentacion/`. |
| **Alex** | Módulo Recepción | `packages/shared`: types + schemas de `habitacion`, `reserva`, `huesped`, `pago`. Contrato de endpoints de reservas/habitaciones/huéspedes (documento). Primer endpoint: `GET/POST /habitaciones`. |
| **Hugo** | Módulo Limpieza + Mantenimiento | `packages/shared`: types + schemas de `limpieza`, `incidencia`, `mantenimiento`. Contrato de endpoints. Primer endpoint: `GET/POST /incidencias`. |
| **Josue** | Módulo Room Service + Inventario | `packages/shared`: types + schemas de `roomservice`, `inventario`, `solicitud`. Contrato de endpoints. Primer endpoint: `GET /roomservice/menu`. |

> Alex, Hugo y Josue empiezan por `packages/shared` y los contratos porque la API y la web base aún no existen; en cuanto Carlos y Kim terminen el scaffold, pasan a construir sus módulos completos.

## Sprint 2 — Módulos core (front + back)
| Persona | Módulo |
|---------|--------|
| Alex | Recepción: habitaciones, reservas, disponibilidad, check-in/out, huéspedes |
| Hugo | Limpieza: mapa, tareas, solicitudes, incidencias, objetos olvidados |
| Josue | Room Service: menú, pedidos, cargos |
| Carlos | Mantenimiento: bandeja de incidencias, órdenes de trabajo, activos, preventivo |
| Kim | Admin: panel de habitaciones, personal, inventario |
| Pablo | Notificaciones (WebSocket), pagos/comprobantes, soporte a todos |

## Sprint 3 — Rotación
Cada quien toma un módulo que **no** hizo y lo completa/pule. Sugerencia:
| Persona | Módulo |
|---------|--------|
| Alex | Portal huésped: reservar, check-in web |
| Hugo | Portal huésped: mi habitación, servicios, cuenta |
| Josue | Admin: tarifas, promociones, reportes |
| Carlos | Room Service + inventario (mejoras, tiempo real) |
| Kim | Recepción (pulir vistas, comprobantes) |
| Pablo | Limpieza + mantenimiento (pulir, tiempo real) |

## Sprint 4 — App móvil (huésped) + cierre
Todos sobre `apps/mobile` por pantallas + pruebas, manuales y despliegue.

## Reglas para que la rotación funcione
1. Todo módulo tiene su contrato de endpoints documentado antes de codificar.
2. Ningún PR se mergea sin revisión de alguien que **no** trabaja en ese módulo.
3. Al rotar, el dueño anterior hace una demo de 15 min al nuevo.
4. Daily de 10 min (puede ser asíncrono por WhatsApp/Discord): qué hice, qué haré, qué me bloquea.
