# Modelo de datos (lenguaje de negocio)

> Versión 1 (2026-10-16). Propuesta para el MVP basada en **práctica general**; nada confirmado aún por el dueño. No es un esquema de base de datos: el constructor lo traduce.
>
> Columna **Fase**: `MVP` = indispensable para la primera versión · `Después` = cuando se pida.

## Mapa de entidades

```
Tercero ──< Compra ──< Partida de compra >── Animal
Tercero ──< Venta  ──< Partida de venta  >── Animal
Animal  ──< Movimiento de inventario
Animal  ──< Pesaje
Animal  >── Lote (opcional)      Animal >── Ubicación     Animal >── Giro
Gasto   ──> (Animal | Lote | Giro | General)
Usuario ──< (todo registro: quién y cuándo)
```

## Catálogos

| Catálogo | Valores iniciales sugeridos | Fase |
|---|---|---|
| Giro | Rancho, Criadero, Compra-venta | MVP (fijo) |
| Categoría | Becerro, Becerra, Torete, Novillo, Vaquilla, Vaca, Toro/Semental | MVP (editable) |
| Raza | Libre/editable (p. ej. Charolais, Beefmaster, Suizo, cruza, criollo) | MVP |
| Ubicación | Rancho/predio → potrero o corral | MVP (2 niveles) |
| Fierro | Fierros del dueño (nombre, imagen opcional, folio de registro) | MVP |
| Categoría de gasto | Alimento, Sanidad, Mano de obra, Flete, Comisión, Combustible, Mantenimiento, Renta, Otros | MVP (editable) |
| Causa de muerte | Libre o lista corta | Después |

## Animal — `MVP`

| Campo | Notas | Fase |
|---|---|---|
| Arete SINIIGA | 10 dígitos, único. Puede faltar temporalmente (cría recién nacida). | MVP |
| Arete interno | Número de manejo. Único entre activos (*por confirmar si se reutiliza*). | MVP |
| Fierro | Referencia a catálogo. | MVP |
| Sexo | Macho / Hembra. | MVP |
| Raza | Catálogo. | MVP |
| Fecha de nacimiento | Real o estimada (marcar si es estimada). | MVP |
| Fecha de ingreso | Fecha de alta en el negocio (compra o nacimiento). | MVP |
| Categoría | Catálogo. Cambia con el tiempo. | MVP |
| Giro actual | Rancho / Criadero / Compra-venta. | MVP |
| Ubicación actual | Potrero/corral. | MVP |
| Lote actual | Opcional. | MVP |
| Madre | Animal (opcional). | MVP (opcional) |
| Padre | Animal o texto (semental externo / pajilla IA). | Después |
| Estado | Activo / Vendido / Muerto / Robado-extraviado / (Traspasado solo como movimiento, no estado final; ver nota). | MVP |
| Color / señas | Texto libre. | MVP (opcional) |
| Foto | | Después |
| Peso de entrada | Se toma del primer pesaje. | MVP (derivado) |
| Costo acumulado | Se calcula desde compras + gastos; no se captura. | MVP (derivado) |
| Notas | | MVP |

> Nota sobre "traspasado": un traspaso entre giros o ubicaciones **no saca** al animal del inventario; el animal sigue *Activo* con otro giro. Solo tendría sentido un estado "Traspasado" si el traspaso es a **otra persona/empresa** relacionada (p. ej. a un familiar). Ver pregunta en `PREGUNTAS.md`.

## Lote — `MVP`

| Campo | Notas |
|---|---|
| Nombre / folio | P. ej. "Compra Tlaltenango 12-oct". |
| Giro | |
| Fecha de creación / cierre | Un lote se "cierra" cuando sale el último animal. |
| Tipo | Compra, Engorda, Venta, Manejo (libre). |
| Cabezas | Derivado de los animales ligados. |
| Modo | **Individual** (cada animal con arete en la app) o **Agrupado** (solo número de cabezas, sin detalle por animal). *Decisión abierta*: el MVP puede empezar solo con "Individual" si el dueño lo confirma. |

## Pesaje — `MVP`

| Campo | Notas |
|---|---|
| Fecha | |
| Animal **o** Lote | Si es de lote: peso total + número de cabezas → peso promedio. |
| Peso (kg) | Decimal con 1 decimal. |
| Tipo | Entrada, Control, Salida. |
| Usuario | |

## Movimiento de inventario — `MVP`

Toda alta, baja o cambio genera un movimiento; las existencias se derivan de aquí.

| Campo | Notas |
|---|---|
| Fecha | |
| Animal | (o lote, en modo agrupado) |
| Tipo | **Alta**: compra, nacimiento, ajuste de inventario inicial. **Baja**: venta, muerte, robo/extravío, consumo propio, ajuste. **Traspaso**: de giro, de ubicación, de lote. **Cambio de categoría**. |
| Origen / destino | Giro, ubicación o lote anterior y nuevo. |
| Documento ligado | Compra, venta o nada. |
| Valor | Para traspasos de giro: valor de traspaso. |
| Motivo / notas | Obligatorio en bajas por muerte, robo o ajuste. |
| Cancela a | Referencia al movimiento que revierte (nunca se borra). |
| Usuario, fecha de captura | Trazabilidad. |

## Compra — `MVP`

| Campo | Notas |
|---|---|
| Fecha | |
| Proveedor | Tercero. |
| Giro destino | Normalmente Compra-venta o Rancho. |
| Modalidad de precio | Por kilo / por cabeza / total del lote. |
| Peso total, merma % | Si es por kilo. |
| Importe del ganado | En centavos. |
| Gastos de compra | Flete, comisión, otros (se ligan como Gasto con asignación a esta compra). |
| Partidas | Animales comprados con su importe individual (calculado o capturado). |
| Forma de pago | Efectivo, transferencia, cheque, crédito. |
| Estado de pago | Pagada / parcial / pendiente. Pagos: `Después` (en MVP basta el estado). |
| CFDI | UUID o "sin factura". |
| Documentos | Guía, factura (archivo adjunto): `Después`. |

## Venta — `MVP`

| Campo | Notas |
|---|---|
| Fecha | |
| Cliente | Tercero. |
| Modalidad de precio | Por kilo / por cabeza / total. |
| Peso total, merma % | |
| Importe | Centavos. |
| Gastos de venta | Flete, comisión. |
| Partidas | Animales vendidos; a cada uno se le asigna su parte del importe (por kilo o por cabeza). |
| Forma y estado de cobro | Igual que compra. |
| CFDI | UUID o "sin factura". |

## Gasto — `MVP`

| Campo | Notas |
|---|---|
| Fecha | |
| Categoría | Catálogo. |
| Concepto | Texto. |
| Proveedor | Tercero (opcional). |
| Importe | Centavos. IVA por separado si hay factura (`Después` si no se factura). |
| Asignación | Animal / Lote / Compra / Venta / Giro / General. |
| Método de prorrateo | Por cabeza (MVP) · por kilo · por cabeza-día (`Después`). |
| Forma de pago, CFDI | |

## Tercero — `MVP`

| Campo | Notas |
|---|---|
| Nombre | |
| Tipo | Proveedor, Cliente, Comisionista, Transportista (puede ser varios). |
| Teléfono, localidad | |
| RFC | Opcional. |
| UPP / PSG | Opcional (útil para trazabilidad). |
| Saldo | Derivado (`Después`, cuando haya cuentas por cobrar/pagar). |

## Usuario — `MVP`

Nombre, usuario, contraseña (segura), rol, activo. Roles iniciales sugeridos: **Dueño/Admin** (todo), **Capturista/Vaquero** (inventario y pesajes, sin ver dinero), **Contador** (contabilidad, solo lectura de inventario). *Por confirmar*.

## Reglas de integridad (negocio)

1. Un animal solo puede estar en una partida de venta activa.
2. No se puede vender, traspasar ni pesar un animal que no esté *Activo*.
3. Arete SINIIGA único en todo el sistema (incluye históricos).
4. Las existencias en una fecha = altas − bajas hasta esa fecha.
5. Todo importe en centavos; nada de decimales flotantes.
6. Todo registro guarda quién lo creó/modificó y cuándo; los contables se cancelan, no se borran.

## Para después (fuera del MVP)

Eventos reproductivos, sanidad por animal, inventario de insumos, cuentas por cobrar/pagar con abonos, adjuntos (fotos, PDFs de guías y facturas), integración con básculas o lectores de arete, modo sin conexión.
