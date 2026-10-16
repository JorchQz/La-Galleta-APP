# Contabilidad del negocio

> Versión 1 (2026-10-16). Es **contabilidad de gestión** (para que el dueño sepa cuánto gana por giro y por lote), no contabilidad fiscal. Lo fiscal está en `NORMATIVA.md`. Donde hay decisión se dan opciones y se remite a `PREGUNTAS.md`.

## 1. Costo por animal

```
Costo acumulado = Costo de entrada + Gastos directos + Gastos prorrateados (+ Indirectos, opcional)
```

| Componente | Cómo se obtiene | Ejemplo |
|---|---|---|
| Costo de entrada (compra) | Importe del ganado repartido entre los animales de la compra. Si es por kilo: peso individual × precio/kg. Si es por cabeza o total del lote: importe ÷ cabezas (o proporcional al peso si hay pesos individuales). | Lote de 20 por $360,000 → $18,000 c/u |
| + Flete y comisión de compra | Se prorratean entre los animales de esa compra (por cabeza). | Flete $8,000 + comisión $4,000 → +$600 c/u |
| Costo de entrada (nacimiento) | Ver decisión D1. | |
| + Gastos directos | Gasto asignado a un animal concreto. | Tratamiento $450 |
| + Gastos de lote | Gasto asignado a un lote, prorrateado entre los animales activos del lote en esa fecha. | Alimento $12,000 / 20 → +$600 c/u |
| + Gastos de giro | Gasto asignado a un giro, prorrateado entre animales activos del giro (ver D3). | |
| Indirectos/generales | Sueldos, luz, mantenimiento. Ver D4. | |

### Métodos de prorrateo

| Método | Cuándo conviene | Fase |
|---|---|---|
| Por cabeza | Simple; vacunas, desparasitantes, fletes. | MVP |
| Por kilo | Cuando los pesos varían mucho (compra por kilo). | MVP para compras |
| Por cabeza-día | Alimento o pastoreo de un periodo, con animales que entran y salen. Es lo más justo. | Después |

## 2. Costo por lote

Costo del lote = suma de costos acumulados de sus animales (o, en modo agrupado, compra + gastos del lote).
Indicadores: costo por cabeza, costo por kg ganado = (gastos posteriores a la compra) ÷ (kg ganados).

## 3. Utilidad

| Nivel | Fórmula |
|---|---|
| Por animal | Precio de venta asignado − gastos de venta prorrateados − costo acumulado. |
| Por lote | Suma de utilidades de sus animales vendidos. Un lote abierto muestra utilidad "estimada" usando precio de mercado (opcional). |
| Por giro (periodo) | Ventas del giro + valor de traspasos que salen − costo de lo vendido − costo de traspasos que entran − gastos del giro no asignados − (muertes) |
| Negocio total | Suma de giros − gastos generales. Los traspasos internos se cancelan entre sí. |

Muertes: el costo acumulado del animal muerto es pérdida del giro donde estaba (opción simple, MVP). Alternativa: repartirlo entre el lote (refleja que la mortalidad es parte del costo del lote).

## 4. Traspasos internos entre giros

Ejemplo: becerro destetado del Criadero pasa al Rancho para engorda.

| Opción | Cómo funciona | Pros | Contras |
|---|---|---|---|
| **A. Al costo acumulado** | El animal pasa con el costo que trae. El Criadero no gana ni pierde. | Sencillo, sin utilidades "ficticias". | El Criadero nunca muestra utilidad por sus crías; no se sabe si criar conviene más que comprar. |
| **B. A precio de mercado** | El Criadero "vende" al Rancho al precio por kilo del día (peso × precio). Utilidad interna para el Criadero; costo de entrada para el Rancho. | Cada giro se evalúa como negocio independiente. Responde "¿me conviene criar o comprar?". | Requiere capturar un precio de referencia; la suma de giros sigue cuadrando, pero hay que explicar la utilidad interna. |
| **C. Precio estándar fijo** | Tabla de precios por categoría/peso definida por el dueño (p. ej. becerro destetado = $X). | Predecible, fácil de capturar. | Hay que actualizar la tabla. |

Recomendación preliminar (no decisión): **B** con sugerencia automática del último precio por kilo de venta/compra registrado, editable. Decide el dueño → **D2**.

Costo de una cría nacida (**D1**):

| Opción | Detalle |
|---|---|
| 0 al nacer | Todos los gastos del criadero (vientres, sementales, alimento) se quedan en el giro Criadero como gasto; la cría vale 0 hasta el traspaso. Más simple. |
| Costo absorbido | Gastos del criadero del periodo ÷ crías destetadas = costo por cría. Más preciso, más trabajo. |

Vientres y sementales comprados: en contabilidad fiscal se deprecian (activo fijo); en gestión, la opción simple es tratarlos como animales del Criadero con su costo de compra, sin depreciación en MVP (*por confirmar con el contador del dueño*).

## 5. Decisiones abiertas (resumen)

| ID | Decisión | Opciones |
|---|---|---|
| D1 | Costo inicial de una cría | 0 / costo absorbido |
| D2 | Valor de traspaso entre giros | Costo / mercado / estándar |
| D3 | Gastos asignados a un giro | Prorratear a animales / dejarlos como gasto del giro en el periodo |
| D4 | Gastos generales | No prorratear (solo en utilidad total) / repartir entre giros por % o por cabezas |
| D5 | Muertes | Pérdida del giro / costo del lote |
| D6 | Pesaje para prorrateo | Por cabeza siempre / por kilo cuando haya peso |

## 6. Reportes que usa un dueño de rancho

| # | Reporte | Contenido | Fase |
|---|---|---|---|
| R1 | Existencias | Cabezas activas por giro × categoría × ubicación, a una fecha. Con total de kg estimado. | MVP |
| R2 | Movimientos del periodo | Altas y bajas por tipo (compras, nacimientos, ventas, muertes, traspasos). "Inventario inicial + entradas − salidas = final". | MVP |
| R3 | Compras del periodo | Por proveedor, cabezas, kg, importe, precio promedio por kg, gastos de compra. | MVP |
| R4 | Ventas del periodo | Por cliente, cabezas, kg, importe, precio promedio por kg. | MVP |
| R5 | Utilidad por lote | Costo, venta, utilidad, % de margen, días en inventario, GDP. | MVP |
| R6 | Utilidad por giro | Estado de resultados simple por giro y total. | MVP |
| R7 | Gastos por categoría | Por periodo y por giro. | MVP |
| R8 | Flujo de efectivo | Entradas (cobros) y salidas (pagos) del periodo, por forma de pago. En MVP con base en fecha de compra/venta/gasto; con abonos: Después. | MVP simple |
| R9 | Ficha del animal | Historial: compra/nacimiento, pesajes, traspasos, gastos, costo acumulado, venta. | MVP |
| R10 | Cuentas por cobrar / pagar | Saldos con terceros. | Después |
| R11 | Mortalidad | % de muertes por giro y periodo. | Después |

Todos los reportes deben poder filtrarse por fecha y giro, y exportarse (PDF/Excel: *por confirmar cuál prefiere el dueño*).
