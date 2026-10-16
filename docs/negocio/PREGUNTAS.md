# Preguntas para el dueño

> Versión 1 (2026-10-16). Preguntas que **solo el dueño puede responder** y que bloquean decisiones de diseño. No se deben suponer respuestas. Cuando se responda una, anotar la respuesta, la fecha y quién respondió, y llevar la decisión a `docs/estado/DECISIONES.md`.
>
> Prioridad: **A** = bloquea el MVP · **B** = importante pero hay valor por defecto razonable · **C** = puede esperar.

Formato de respuesta: `R: ... (fecha, quién)`

## 1. Negocio y escala

| # | Pregunta | Prioridad | Respuesta |
|---|---|---|---|
| 1 | ¿Dónde está(n) el/los rancho(s)? ¿Es región Colotlán, norte de Jalisco? ¿Cuántos predios y cuántas UPP tienen? | A | |
| 2 | ¿Cuántos animales hay hoy aprox. por giro (rancho, criadero, compra-venta)? ¿Cuántos entran y salen al mes en compra-venta? | A | |
| 3 | ¿El negocio es de una sola persona o hay socios/familiares? ¿Cada giro tiene dueño distinto? | A | |
| 4 | ¿Cada animal pertenece siempre a un solo giro? ¿Hay animales "a medias" o de terceros (a pastoreo, en aparcería)? | A | |
| 5 | ¿Solo bovinos o también otras especies (caballos, borregos)? | B | |

## 2. Inventario

| # | Pregunta | Prioridad | Respuesta |
|---|---|---|---|
| 6 | En compra-venta, ¿se registra **cada animal** con su arete o basta con el **lote** (cabezas y kilos)? | A | |
| 7 | ¿Todos los animales tienen arete SINIIGA? ¿Usan además arete interno? ¿Se reutilizan números internos? | A | |
| 8 | ¿Cuántos fierros usan? ¿Están registrados? ¿El ganado comprado se vuelve a herrar? | B | |
| 9 | ¿Qué categorías usan en el día a día (becerro, torete, novillo, vaquilla, vaca, toro…)? ¿Cuándo cambia un animal de categoría? | A | |
| 10 | ¿Cómo dividen ubicaciones: rancho → potrero → corral? ¿Necesitan saber en qué potrero está cada animal? | B | |
| 11 | ¿Pesan a los animales? ¿Individual o por lote? ¿Al comprar, al vender y/o periódicamente? ¿Tienen báscula? | A | |
| 12 | ¿Quieren registrar madre de cada cría? ¿Y padre/semental? | B | |
| 13 | "Traspasado" como estado final: ¿hay traspasos a otras personas/empresas (familiares, socios) además de entre giros? | B | |
| 14 | ¿Llevan registro de muertes con causa? ¿Dan de baja el arete en SINIIGA? | C | |

## 3. Compras y ventas

| # | Pregunta | Prioridad | Respuesta |
|---|---|---|---|
| 15 | ¿Compran/venden por kilo, por cabeza o ambas? ¿Manejan merma pactada (qué %)? | A | |
| 16 | ¿A quién compran y venden normalmente (subastas, ranchos, engordas, rastros, exportación)? | B | |
| 17 | ¿Pagan comisión a intermediarios? ¿Cómo se calcula (por cabeza, %)? ¿Quién paga el flete? | A | |
| 18 | ¿Venden a crédito o compran a crédito? ¿Necesitan controlar saldos/abonos desde el inicio? | B | |
| 19 | ¿Cuánto tiempo suele quedarse el ganado de compra-venta (días, semanas, más de 3 meses)? | B | |

## 4. Contabilidad (ver decisiones D1–D6 en `CONTABILIDAD.md`)

| # | Pregunta | Prioridad | Respuesta |
|---|---|---|---|
| 20 | **D2:** Cuando una cría del criadero pasa a engorda o a venta, ¿a qué valor se traspasa: costo acumulado, precio de mercado del día o un precio fijo por categoría? | A | |
| 21 | **D1:** ¿Quieren saber cuánto les cuesta producir cada becerro (prorratear gastos del criadero) o basta ver la utilidad del criadero en conjunto? | A | |
| 22 | **D4:** Sueldos, luz, combustible y gastos generales, ¿se reparten entre los giros? ¿Con qué criterio (% fijo, por cabezas)? | A | |
| 23 | **D5:** Una muerte, ¿es pérdida del giro o se carga al lote? | B | |
| 24 | ¿Cómo llevan hoy las cuentas (libreta, Excel, LaGRapp, contador externo)? ¿Hay datos que haya que importar? | A | |
| 25 | ¿Qué reportes revisan hoy y con qué frecuencia? ¿Cuál es la pregunta que más quieren que la app conteste (p. ej. "¿cuánto gané con este lote?")? | A | |
| 26 | ¿Prefieren exportar reportes a PDF, Excel o ambos? | C | |

## 5. Fiscal

| # | Pregunta | Prioridad | Respuesta |
|---|---|---|---|
| 27 | ¿El negocio está a nombre de persona física, persona moral o varias? ¿Qué régimen fiscal tiene (AGAPES, RESICO, otro)? | A | |
| 28 | ¿Facturan todas las ventas? ¿Reciben factura en las compras de ganado? ¿Qué parte se maneja sin factura? | A | |
| 29 | ¿Quieren que la app emita facturas (CFDI) o solo guardar el folio/UUID de las que hacen en otro sistema? | B | |
| 30 | ¿Tienen contador? ¿Debe tener acceso a la app? | B | |

## 6. Usuarios y uso

| # | Pregunta | Prioridad | Respuesta |
|---|---|---|---|
| 31 | ¿Cuántas personas usarán la app y qué hace cada una (dueño, encargado, vaquero, contador)? | A | |
| 32 | ¿Hay información que algunos usuarios **no** deben ver (precios, utilidades)? | A | |
| 33 | ¿Se usará desde el celular en el corral/potrero? ¿Hay señal ahí o se necesita capturar sin conexión y sincronizar después? | A | |
| 34 | ¿Qué celulares/computadoras usan? ¿Hay lector de aretes o báscula electrónica que convenga conectar después? | C | |
| 35 | ¿Qué es lo que más les molesta de LaGRapp (lento, demasiadas pantallas, funciones que no usan)? ¿Qué sí les gusta? | A | |
