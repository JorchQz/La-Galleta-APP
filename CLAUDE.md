# La Galleta APP

Webapp (usuario y contraseña) para controlar un negocio ganadero con tres giros:

1. **Rancho** – ganado propio en engorda/pastoreo.
2. **Criadero** – reproducción (vientres, sementales, partos, crías).
3. **Compra-venta** – ganado que entra y sale para reventa.

Alcance deliberadamente reducido. Solo dos módulos núcleo:

- **Inventario de ganado** – alta, movimientos, bajas y existencias por animal y por lote.
- **Contabilidad** – compras, ventas, gastos, costo por animal/lote y utilidad por giro.

Todo lo demás queda fuera hasta que se documente lo contrario en `docs/estado/DECISIONES.md`.

## Referencias externas (solo lectura)

- `ServiciosMana/Sistema-SM`: sistema que funciona fluido. **No se modifica jamás.** Solo se estudia para copiar lo que funciona (arquitectura, patrones, rendimiento).
- `Guayaba13/LaGRapp`: base funcional del programa actual (TypeScript). Se considera lento/tosco y con funciones de sobra. Esta app es su versión mejorada.

Ambos repos son privados. Para leerlos, la sesión necesita acceso de GitHub a esas cuentas/organizaciones (ver `docs/estado/ESTADO.md` → Bloqueos).

## Memoria del proyecto: TODO vive en archivos

Este proyecto lo trabajan dos personas en distintas sesiones. **Las conversaciones no son memoria.** Al empezar cualquier sesión:

1. Lee `docs/estado/ESTADO.md` (dónde estamos, qué sigue, bloqueos).
2. Lee `docs/estado/TAREAS.md` (backlog priorizado).
3. Revisa las últimas entradas de `docs/estado/BITACORA.md`.

Al terminar cualquier sesión con cambios:

1. Actualiza `ESTADO.md` y `TAREAS.md`.
2. Agrega una entrada al **inicio** de `BITACORA.md` (fecha, quién, qué se hizo, qué quedó pendiente).
3. Si se tomó una decisión de arquitectura/negocio, regístrala en `DECISIONES.md`.
4. Haz commit y push.

| Archivo | Propósito |
|---|---|
| `docs/estado/ESTADO.md` | Foto actual del proyecto. Se sobrescribe. |
| `docs/estado/TAREAS.md` | Backlog con dueño (agente) y estado. |
| `docs/estado/BITACORA.md` | Historial de sesiones. Solo se agrega, nunca se borra. |
| `docs/estado/DECISIONES.md` | Registro de decisiones (ADR). Solo se agrega. |
| `docs/negocio/` | Conocimiento del negocio (dueño: investigador). |
| `docs/seguridad/` | Modelo de amenazas, checklist y auditorías (dueño: seguridad). |
| `docs/agentes/REGISTRO.md` | Lista de agentes existentes y cuándo usarlos. |

## Equipo de agentes

La sesión principal actúa como **Maestro** (ver skill `/maestro`). El Maestro no programa directamente tareas grandes: planea, delega y verifica.

| Agente | Archivo | Rol |
|---|---|---|
| Maestro | `.claude/skills/maestro/SKILL.md` | Orquesta, prioriza, crea agentes nuevos, mantiene el estado. |
| Investigador | `.claude/agents/investigador.md` | Experto del negocio ganadero y contable. |
| Constructor | `.claude/agents/constructor.md` | Escribe el código. |
| Seguridad | `.claude/agents/seguridad.md` | Revisa autenticación, datos, permisos y dependencias. |

Los subagentes no pueden lanzar otros subagentes; por eso el Maestro es la sesión principal. Para crear un agente nuevo, el Maestro usa `docs/agentes/PLANTILLA.md` y lo registra en `docs/agentes/REGISTRO.md`.

## Reglas para el código

- Idioma de la interfaz y de los términos de negocio: español (México). Código e identificadores: inglés o español, pero consistente con lo que ya exista.
- Dinero: nunca `float`. Guardar en centavos (entero) o `numeric` en la BD.
- Toda operación contable debe ser trazable: quién, cuándo, qué cambió. No se borran movimientos contables; se cancelan con un movimiento inverso.
- Rendimiento es requisito, no extra: paginar listas, indexar búsquedas, evitar cargar datos que la pantalla no muestra.
- Ningún secreto (contraseñas, llaves, tokens) en el repositorio.
