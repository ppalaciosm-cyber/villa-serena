# ADR-001: Stack tecnológico

**Estado:** Aceptado · **Fecha:** 2026-09-11

## Contexto
Prototipo de UI ya hecho en React + Vite + Tailwind. Se necesita backend, base de datos y, más adelante, app móvil para huéspedes.

## Decisión
- Monorepo con pnpm workspaces.
- Frontend: React 19 + Vite + TypeScript + Tailwind v4 (se conserva el prototipo).
- Backend: NestJS + Prisma + PostgreSQL.
- Auth: JWT con roles.
- Tiempo real: Socket.IO.
- Móvil: React Native + Expo (fase posterior).

## Consecuencias
- Un solo lenguaje (TypeScript) en todo el proyecto; tipos compartidos en `packages/shared`.
- Curva de aprendizaje de NestJS y Prisma para quienes no los conocen: se dedicará parte del Sprint 1 a onboarding.
- Ver `docs/arquitectura/stack.md` para el detalle completo.
