# Procesos del negocio

> Versión 1 (2026-10-16). Flujos de **práctica general**; ninguno confirmado aún por el dueño. Cada proceso indica qué registra la app (inventario y/o contabilidad). Las dudas están en `PREGUNTAS.md`.

Convención: **[INV]** = efecto en inventario · **[CONT]** = efecto contable.

## 1. Compra de ganado

1. Se negocia con el vendedor: precio por kilo o por cabeza, merma pactada, quién paga flete.
2. Se pesa (individual o el lote en báscula) en origen o al llegar.
3. Se revisan aretes SINIIGA y documentos (guía/factura del vendedor).
4. Se transporta (flete) y se paga comisión si hubo intermediario.
5. Llegan los animales: se les asigna arete interno, se herran si aplica, se asignan a giro y ubicación.

| Qué registra la app | |
|---|---|
| [INV] | Alta de cada animal (o del lote) con motivo "compra", giro, ubicación, peso de entrada. |
| [CONT] | Compra (proveedor, fecha, importe, forma de pago, CFDI si hay) + gastos ligados (flete, comisión). Se calcula costo de entrada por animal. |

## 2. Nacimiento (criadero)

1. Pare la vaca; se registra fecha, sexo y madre (padre si se conoce).
2. Se coloca arete (interno al nacer; SINIIGA normalmente antes del destete o de la primera movilización — *por confirmar práctica del dueño*).
3. La cría queda en el giro **Criadero** hasta el destete.

| | |
|---|---|
| [INV] | Alta con motivo "nacimiento", madre, giro Criadero. |
| [CONT] | Sin compra. Costo inicial = 0 o costo estándar (decisión abierta, ver `CONTABILIDAD.md`). |

## 3. Pesaje

1. Se pesa un animal o el lote completo.
2. Se registra fecha, peso y, si es por lote, número de cabezas.

| | |
|---|---|
| [INV] | Pesaje ligado al animal o al lote. Permite calcular GDP. |
| [CONT] | Ninguno directo; sirve para prorratear por kilo y valuar existencias. |

## 4. Gasto (alimento, sanidad, mano de obra, etc.)

1. Se paga el gasto (con o sin factura).
2. Se indica a qué se asigna: un animal, un lote, un giro o general.

| | |
|---|---|
| [CONT] | Gasto con categoría, importe, proveedor, asignación. Si es a lote o giro, se prorratea a los animales. |

## 5. Venta de ganado

1. Se negocia con el comprador (por kilo o por cabeza, merma).
2. Se pesa y se selecciona qué animales salen.
3. Se emite guía/documentos de movilización y CFDI si aplica.
4. Se cobra (contado, crédito, parcial).

| | |
|---|---|
| [INV] | Baja de cada animal con motivo "venta", fecha y peso de salida. |
| [CONT] | Venta (cliente, importe, forma de cobro, CFDI). Se calcula utilidad = precio − costo acumulado de cada animal. Gastos de venta (flete, comisión) restan a la utilidad. |

## 6. Muerte

1. Se detecta el animal muerto; se registra causa probable y fecha.
2. Se reporta (si aplica) para dar de baja el arete SINIIGA (*por confirmar si el dueño lo hace*).

| | |
|---|---|
| [INV] | Baja con motivo "muerte". |
| [CONT] | El costo acumulado del animal se reconoce como pérdida del giro (o se reparte entre el lote; decisión abierta). |

## 7. Robo / extravío

Igual que muerte, con motivo "robo" o "extravío". Debe poder revertirse si el animal aparece (con un movimiento inverso, nunca borrando).

## 8. Traspaso entre giros

Ejemplos: becerro destetado del Criadero → Rancho (engorda); vaquilla del Rancho → Criadero (reemplazo); animal de Compra-venta que se queda a engordar → Rancho.

1. Se elige animal(es), giro destino y fecha.
2. Se fija el **valor de traspaso** (costo acumulado o precio de mercado; decisión abierta).

| | |
|---|---|
| [INV] | Movimiento "traspaso de giro". El animal sigue activo; cambia su giro. |
| [CONT] | Asiento interno: el giro origen "vende" y el destino "compra" al valor de traspaso. No es ingreso real ni genera CFDI. |

## 9. Traspaso de ubicación

Cambio de potrero/corral/rancho. [INV] solamente. Si es entre predios con distinta UPP, puede requerir documentos de movilización (*por confirmar*).

## 10. Cambio de categoría

Becerro → novillo/torete al destete; vaquilla → vaca al primer parto; torete → semental. Puede ser automático (por edad o evento) o manual. [INV] solamente.

## 11. Cancelación / corrección

Ningún movimiento se borra. Un error se corrige con un movimiento inverso (p. ej. "cancelación de venta") que queda registrado con usuario, fecha y motivo.

## Fuera del MVP (documentado para después)

- Control reproductivo detallado (empadres, IA, diagnósticos, intervalos entre partos).
- Calendario sanitario y vacunas por animal.
- Inventario de insumos (alimento, medicinas).
- Nómina.
