# Modelo de amenazas – La Galleta APP

Versión inicial: 2026-10-16 · Dueño: agente seguridad · Independiente del stack (ADR-002 pendiente). Revisar cuando se decida el stack y cuando exista código.

**Principio rector:** en esta app el ganado es dinero. Que alguien **altere** existencias o movimientos sin dejar rastro es tan grave como que alguien **lea** los datos. La integridad va primero.

---

## 1. Activos

| # | Activo | Por qué importa | Propiedad clave |
|---|---|---|---|
| A1 | Inventario de ganado (animales, lotes, existencias, altas/bajas, traspasos entre giros) | Es el patrimonio. Un faltante oculto = dinero perdido. | Integridad |
| A2 | Movimientos contables (compras, ventas, gastos, cancelaciones) | Base de costo por animal/lote y utilidad por giro. | Integridad, trazabilidad |
| A3 | Bitácora de auditoría | Única prueba de quién hizo qué. Si se puede borrar, no sirve. | Integridad, disponibilidad |
| A4 | Credenciales y sesiones (contraseñas, cookies/tokens) | Llave a todo lo anterior. | Confidencialidad |
| A5 | Secretos del sistema (llave de BD, llave de servicio, claves de API) | Saltan cualquier permiso de la app. | Confidencialidad |
| A6 | Respaldos | Recuperación ante borrado, error o ataque. También contienen todo el patrimonio. | Disponibilidad, confidencialidad |
| A7 | Datos de terceros (proveedores, compradores, precios) | Información comercial sensible. | Confidencialidad |

## 2. Actores

| Actor | Qué puede querer | Acceso de partida |
|---|---|---|
| **Usuario interno honesto** | Capturar rápido; puede equivocarse. | Su rol. |
| **Empleado deshonesto** | Ocultar una venta en efectivo, un robo de ganado, inflar un gasto o mover un animal a "muerte" para cubrir una venta por fuera. | Su rol (normalmente capturista). **Es la amenaza más probable.** |
| **Usuario con privilegio alto** (admin) que se equivoca o abusa | Corregir "a mano" borrando registros. | Admin. |
| **Atacante externo** | Robar datos, cifrar/borrar (ransomware), usar credenciales filtradas. | Ninguno; internet. |
| **Pérdida o robo de dispositivo** (celular/laptop en el rancho) | Quien lo encuentre entra con la sesión abierta. | Sesión activa del dueño del equipo. |
| **Desarrollador / agente con acceso al repo** | Sin mala intención, sube un secreto o abre una política de más. | Repo, despliegue. |

## 3. Amenazas priorizadas

Escala: **C** crítica · **A** alta · **M** media · **B** baja.

| # | Prioridad | Amenaza | Escenario concreto | Controles (ver §4) |
|---|---|---|---|---|
| T1 | **C** | Editar o borrar un movimiento para ocultar un faltante | Capturista vende 3 becerros en efectivo, luego borra la venta o cambia la cantidad de 5 a 2. | C1, C2, C3, C4 |
| T2 | **C** | Baja falsa de ganado | Se registra "muerte" o "extravío" de un animal que en realidad se vendió por fuera. | C2, C5, C6 |
| T3 | **C** | Autorización solo en el cliente | La pantalla oculta el botón "borrar", pero la API/BD lo acepta si se llama directo (o falta RLS en una tabla). | C7, C8 |
| T4 | **C** | Manipular totales/precios desde el cliente | El navegador envía el total o el costo calculado; se altera antes de mandarlo. | C9 |
| T5 | **A** | Borrar o alterar la bitácora | Quien hizo el cambio borra su rastro. | C3, C10 |
| T6 | **A** | Robo de credenciales / fuerza bruta | Contraseña débil o reutilizada; sin límite de intentos. | C11, C12 |
| T7 | **A** | Secreto expuesto | Llave de servicio en el repo, en `.env` subido o en variable pública del bundle (`NEXT_PUBLIC_`, `VITE_`). Da acceso total saltándose RLS. | C13 |
| T8 | **A** | Pérdida de datos sin respaldo restaurable | Borrado accidental, proveedor caído o ransomware; el respaldo nunca se probó. | C14 |
| T9 | **A** | Sesión abierta en dispositivo perdido | Encuentran el celular con la app abierta y capturan/consultan. | C12, C15 |
| T10 | **M** | Inyección / XSS / CSRF | Nombre de animal con script; consulta armada con texto del usuario. | C16 |
| T11 | **M** | Fecha retroactiva | Se captura un movimiento con fecha de un periodo ya revisado para cuadrar números. | C3, C17 |
| T12 | **M** | Dependencia vulnerable | Paquete con CVE conocido o abandonado. | C18 |
| T13 | **B** | Fuga de datos comerciales por rol de lectura amplio | El contador ve más de lo necesario. | C7 |

## 4. Controles

### Integridad (lo más importante)
- **C1 – Movimientos inmutables.** Inventario y contabilidad son de *solo agregar*. No hay UPDATE ni DELETE sobre movimientos; un error se corrige con un **movimiento de cancelación** que referencia al original y lleva motivo obligatorio. Las existencias se **calculan** desde movimientos (o se guardan en caché recalculable), nunca se editan directo. Reforzar en la BD (permisos/trigger), no solo en la app.
- **C2 – Separación de funciones.** Quien captura una baja (muerte/extravío) o cancela un movimiento no es quien la aprueba. Bajas no-venta y cancelaciones quedan "pendientes" hasta que dueño/admin confirme.
- **C3 – Bitácora de auditoría.** Cada operación registra: usuario, fecha/hora del **servidor**, acción, entidad, valores antes/después, IP/dispositivo. Se escribe en la misma transacción que la operación.
- **C4 – Conciliación periódica.** Reporte de existencias teóricas vs. conteo físico; diferencias quedan registradas como movimiento de ajuste aprobado, nunca editando el saldo.
- **C5 – Evidencia en bajas.** Baja por muerte/extravío exige motivo y, idealmente, foto/nota; reporte de bajas por usuario y periodo para detectar patrones.
- **C6 – Identificación del animal.** Arete/ID único; no se reutiliza un ID dado de baja.
- **C9 – Cálculos en servidor.** Totales, costos y utilidad se calculan en servidor/BD a partir de cantidad × precio. El cliente solo envía datos de captura. Dinero en centavos (entero) o `numeric`.
- **C10 – Bitácora protegida.** Nadie (ni admin desde la app) puede editar o borrar la bitácora; solo insertar y leer. Se incluye en los respaldos.
- **C17 – Cierre de periodo.** Periodos cerrados no aceptan movimientos nuevos con fecha dentro de ellos, salvo dueño y con bitácora.

### Acceso
- **C7 – Roles mínimos** (ver §5). Mínimo privilegio por defecto.
- **C8 – Autorización en servidor.** Cada endpoint/consulta verifica rol. Si es Postgres/Supabase: **RLS activado en todas las tablas**, políticas por rol, y pruebas que intenten la operación prohibida con cada rol. La llave de servicio nunca llega al cliente.
- **C11 – Contraseñas.** Hash fuerte (argon2id/bcrypt o el del proveedor de auth), longitud mínima 12, rechazo de contraseñas filtradas si el proveedor lo permite, límite de intentos y bloqueo temporal, recuperación por enlace de un solo uso con expiración. MFA recomendado para dueño/admin.
- **C12 – Sesiones.** Cookies `HttpOnly`, `Secure`, `SameSite=Lax/Strict`; expiración por inactividad (propuesta: 12 h capturista, 30 min en acciones de admin con reautenticación); cierre de sesión que invalida en servidor.
- **C15 – Revocar dispositivos.** El dueño puede cerrar todas las sesiones de un usuario y desactivar la cuenta en un clic.

### Plataforma
- **C13 – Secretos.** Solo en variables de entorno del proveedor; `.env*` en `.gitignore`; escaneo de secretos en el repo y en el historial; nada sensible en variables públicas del bundle. Rotar si alguno se expone.
- **C14 – Respaldos.** Automáticos diarios, copia fuera del proveedor principal, cifrados, retención mínima 30 días, y **prueba de restauración** documentada al menos mensual y antes de migraciones.
- **C16 – Entradas.** Validación en servidor, consultas parametrizadas, escape de salida, protección CSRF si se usan cookies.
- **C18 – Dependencias.** Auditoría (`npm audit` o equivalente) en cada despliegue; quitar paquetes que no se usan.
- HTTPS obligatorio con HSTS.

## 5. Roles propuestos — ⚠️ PENDIENTE DE CONFIRMAR CON EL USUARIO

| Rol | Puede | No puede |
|---|---|---|
| **Dueño / Admin** | Todo lo de abajo; aprobar bajas y cancelaciones; cerrar periodos; administrar usuarios; ver bitácora completa. | Editar/borrar movimientos o bitácora (nadie puede). |
| **Capturista** | Registrar altas, compras, ventas, gastos, traspasos; solicitar bajas y cancelaciones; ver existencias del/los giro(s) asignados. | Aprobar sus propias bajas/cancelaciones; administrar usuarios; cerrar periodos; ver bitácora de otros. |
| **Solo lectura / Contador** | Consultar movimientos, reportes y existencias; exportar. | Capturar o modificar cualquier cosa. |

Preguntas para el usuario:
1. ¿Cuántas personas usarán la app y quién hará cada rol?
2. ¿Hace falta limitar capturistas por giro (rancho / criadero / compra-venta)?
3. ¿Quién aprueba bajas por muerte/extravío cuando el dueño no está?
4. ¿El contador necesita ver precios de compra/venta o solo totales?
5. ¿Se usará en celulares compartidos en el rancho? (afecta expiración de sesión)

## 6. Fuera de alcance por ahora
Ataques físicos al servidor del proveedor, amenazas de nivel estatal, cumplimiento fiscal formal (CFDI/SAT) — se reevalúa si entra al alcance vía `DECISIONES.md`.
