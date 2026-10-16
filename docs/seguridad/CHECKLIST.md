# Checklist de seguridad antes de cada despliegue

Versión inicial: 2026-10-16 · Dueño: agente seguridad · Referencias: `MODELO_AMENAZAS.md` (controles C1–C18).

Cómo usarlo: copiar la lista en la auditoría del despliegue (`AUDITORIAS/AAAA-MM-DD-<tema>.md`) y marcar cada punto con ✅, ❌ o N/A + nota. **Cualquier ❌ en un punto marcado 🔴 bloquea el despliegue.**

## Autenticación (C11)
- [ ] 🔴 Contraseñas con hash fuerte (argon2id/bcrypt o proveedor de auth); nunca en texto plano ni en logs.
- [ ] Longitud mínima 12; límite de intentos con bloqueo temporal probado.
- [ ] Recuperación de contraseña por enlace de un solo uso que expira; no revela si el usuario existe.
- [ ] No hay usuarios de prueba ni contraseñas por defecto en producción.

## Sesiones (C12, C15)
- [ ] 🔴 Cookies `HttpOnly`, `Secure`, `SameSite`; tokens fuera de `localStorage`.
- [ ] Expiración por inactividad configurada; cerrar sesión invalida en servidor.
- [ ] El dueño puede desactivar un usuario y cerrar todas sus sesiones.

## Autorización (C7, C8)
- [ ] 🔴 Cada endpoint/consulta valida el rol en el servidor (no solo ocultar botones).
- [ ] 🔴 Si es Postgres/Supabase: RLS **activado en todas las tablas** (incluidas las nuevas de este despliegue) y sin políticas `using (true)` sin justificar.
- [ ] Prueba: con rol capturista y solo lectura, intentar por API directa una operación prohibida → debe fallar.
- [ ] La llave de servicio / `service_role` solo se usa en servidor.

## Integridad contable y de inventario (C1–C4, C9, C10, C17)
- [ ] 🔴 No existe ruta (UI, API ni permiso de BD) para editar o borrar movimientos; correcciones = movimiento de cancelación con motivo.
- [ ] 🔴 Totales, costos y existencias se calculan en servidor/BD; el cliente no los envía como dato confiable.
- [ ] 🔴 Bitácora de auditoría registra usuario, hora del servidor, acción y antes/después; solo insertar, nadie borra.
- [ ] Bajas por muerte/extravío y cancelaciones requieren aprobación de dueño/admin.
- [ ] Periodos cerrados rechazan movimientos con fecha dentro de ellos.
- [ ] Dinero en centavos (entero) o `numeric`; ningún `float`.

## Entradas (C16)
- [ ] Validación en servidor de todos los campos (tipos, rangos, cantidades > 0).
- [ ] Consultas parametrizadas; sin SQL armado con texto del usuario.
- [ ] Salida escapada (XSS); protección CSRF si hay cookies de sesión.

## Secretos (C13)
- [ ] 🔴 Ningún secreto en el repo ni en el historial de git (correr escaneo de secretos).
- [ ] 🔴 Nada sensible en variables públicas del bundle (`NEXT_PUBLIC_`, `VITE_`, etc.).
- [ ] `.env*` en `.gitignore`; secretos solo en el proveedor de despliegue.

## Dependencias (C18)
- [ ] `npm audit` (o equivalente) sin vulnerabilidades altas/críticas sin justificar.
- [ ] Sin paquetes nuevos innecesarios; lockfile comprometido en el repo.

## Respaldos (C14)
- [ ] 🔴 Respaldo automático activo y uno reciente (< 24 h) verificado antes de desplegar migraciones.
- [ ] Prueba de restauración hecha en el último mes (fecha y resultado en la auditoría).
- [ ] Respaldo cifrado y con copia fuera del proveedor principal.

## Transporte y despliegue
- [ ] 🔴 Solo HTTPS; HTTP redirige; HSTS activo.
- [ ] Modo debug apagado; errores al usuario sin trazas ni detalles de BD.
- [ ] Encabezados básicos: `Content-Security-Policy`, `X-Content-Type-Options`, `frame-ancestors`/`X-Frame-Options`.
