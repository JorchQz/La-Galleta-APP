# Estado del proyecto

_Última actualización: 2026-10-16_

## Fase actual

**Fase 0 – Cimientos.** Ya existen el equipo de agentes y los archivos de estado. Todavía no hay código de la app.

## Hecho

- Estructura de agentes: Maestro (`/maestro`), investigador, constructor y seguridad.
- Archivos de estado compartido (`docs/estado/`).
- Investigación inicial del negocio (ver `docs/negocio/`).

## Siguiente

1. Dar acceso a los repos de referencia (ver Bloqueos).
2. Investigador: analizar `LaGRapp` (qué funciones se quedan y cuáles sobran) y `Sistema-SM` (por qué es fluido).
3. Usuario: responder `docs/negocio/PREGUNTAS.md`.
4. Maestro: proponer el stack definitivo en `DECISIONES.md` con base en lo anterior.
5. Constructor: crear el esqueleto de la app con login.

## Bloqueos

- **Sin acceso a `ServiciosMana/Sistema-SM` ni a `Guayaba13/LaGRapp`.** La sesión de Claude solo ve los repos de la cuenta `JorchQz`. Opciones:
  - Que los dueños agreguen a `JorchQz` como colaborador (lectura basta) **e** instalar la GitHub App de Claude en esas cuentas/organizaciones: https://github.com/apps/claude/installations/select_target
  - O copiar el código a un repo bajo `JorchQz`.
  - Después hay que iniciar una sesión nueva con esos repos seleccionados (o agregarlos con `add_repo`).
