# Registro de decisiones

Formato: contexto → decisión → consecuencias. Solo se agregan entradas; si una decisión cambia, se crea una nueva que la reemplace.

---

## ADR-001 – Alcance, memoria en archivos y equipo de agentes (2026-10-16)

**Contexto.** El programa actual (`LaGRapp`) se siente lento/tosco y tiene funciones que no se usan. El proyecto lo llevan dos personas en sesiones distintas.

**Decisión.**
- Alcance inicial: solo **inventario de ganado** y **contabilidad**, para los giros rancho, criadero y compra-venta.
- Webapp con usuario y contraseña.
- El estado del proyecto vive en `docs/estado/` y se actualiza en cada sesión.
- Equipo: Maestro (sesión principal, skill `/maestro`), investigador, constructor y seguridad. El Maestro crea agentes nuevos con `docs/agentes/PLANTILLA.md`.
- `ServiciosMana/Sistema-SM` es solo referencia y no se modifica.

**Consecuencias.** Cualquier función fuera del alcance necesita una nueva entrada aquí.

---

## ADR-002 – Stack tecnológico (PENDIENTE)

Se decide cuando se hayan analizado `LaGRapp` y `Sistema-SM`.

Premisa ya aclarada: **TypeScript no hace más lenta una app.** TypeScript se convierte en JavaScript al compilar, y el navegador y el servidor ejecutan exactamente el mismo JavaScript. La lentitud viene de otras cosas: arquitectura, consultas a la base de datos, cuánto JavaScript se descarga, renderizado de listas grandes, región del servidor, arranques en frío. Recomendación preliminar: **conservar TypeScript**, porque ayuda a evitar errores en cálculos contables y facilita trabajar entre dos personas.
