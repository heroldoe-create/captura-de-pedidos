---
name: captura-de-pedidos
description: >
  Lee un pedido que llega por texto de WhatsApp, correo o PDF; extrae cliente, SKUs, cantidades,
  precios y fecha de entrega; lo empata con el catálogo y la lista de precios del cliente (CSV);
  lo deja como renglones listos para copiar al ERP (CSV) y genera las preguntas al vendedor
  para lo dudoso (SKU ambiguo, precio distinto, cantidad rara). No inventa SKUs: lo que no
  empata va a preguntas.
license: LicenseRef-Heroldo-1.0
---

# Captura de pedidos: WhatsApp / correo / PDF → renglones del ERP

## Qué hace

Convierte un pedido libre (mensaje de WhatsApp, cuerpo de correo o PDF) en dos entregables:

1. **renglones_listos_erp.csv** — un renglón por línea de pedido, empatada con el catálogo y la
   lista de precios del cliente, listo para copiar al ERP (con su plantilla, en sus columnas).
2. **preguntas_al_vendedor.md** — todo lo que no empata, con la pregunta exacta que le toca
   responder al vendedor antes de capturar.

La regla central: **lo que no empata no se adivina: va a preguntas.** El agente cruza, marca
diferencias y arma evidencia; no decide por el cliente ni por el vendedor.

## Entradas que acepta

- Texto de WhatsApp (pegado del mensaje o exportación).
- Cuerpo de correo con el pedido en texto.
- PDF de pedido (orden de compra escaneada o digital). Se transcribe primero a texto y se
  procesa igual; lo ilegible va a preguntas como «PDF ilegible en la línea 3».

## Qué necesita del usuario (sin esto no corre)

1. **El pedido** en cualquiera de las formas de arriba.
2. **Catálogo del vendedor** (CSV): `sku, descripcion, unidad_venta, codigo_barras, activo` y, opcional, `notas` (conversiones de empaque).
3. **Lista de precios del cliente** (CSV): `cliente_id, sku, precio, moneda, vigente_desde`.
4. **Formato de importación del ERP** (CSV de ejemplo o lista de columnas). Si no hay,
   sale con las columnas de la Salida 1 y la pregunta final lo dice: "Formato genérico: ajusta los nombres de columna a tu
   ERP".
5. **Fecha del pedido**: si el mensaje no la trae, se usa la fecha de hoy y se escribe cuál se
   usó en el encabezado de preguntas.

## El pipeline (en orden, no se salta un paso)

1. **Transcribir** el pedido a texto plano. Nada se interpreta todavía.
2. **Segmentar líneas** del pedido: una línea del mensaje = un candidato a renglón. Si una línea
   trae varios productos, se separa; si dos líneas traen el mismo producto, se marcan como
   posible duplicado (no se suman solas). Si la línea trae una alternativa («si no hay, …»,
   «o en su lugar …»), **no se captura ninguna de las dos**: la línea va a preguntas con el tipo
   `alternativa condicionada` y ambas opciones. La alternativa queda junta como una sola línea
   pendiente: nunca son dos pedidos, ni se sustituye una opción por existencia que no se
   proporcionó.
3. **Extraer campos** de cada línea: `descripcion_dicha`, `cantidad_dicha`, `unidad_dicha`,
   `precio_dicho` (si viene), `fecha_entrega_dicha` (si viene).
4. **Empatar con el catálogo:**
   - Match exacto por `sku` (si el cliente escribe el SKU).
   - Match por `codigo_barras`.
   - Match por descripción: normalizar (minúsculas, sin acentos, singular) y comparar contra
     `descripcion` del catálogo. Si hay **un solo** candidato con alta similitud → match.
   - Si hay **dos o más** candidatos razonables → **SKU ambiguo**, va a preguntas con las
     opciones listadas.
   - Si no hay candidato → va a preguntas como «no encontrado en catálogo». **Prohibido crear
     un SKU nuevo o asignar el más parecido por encima.**
5. **Empatar con la lista de precios del cliente:** el SKU empatado se cruza contra la lista del
   `cliente_id` del pedido. Solo vale un SKU con `activo` = `si`; con cualquier otro valor queda
   pendiente aunque el código coincida. Para cada cliente/SKU/moneda se elige la tarifa de mayor
   `vigente_desde` que no sea posterior a la fecha de referencia; dos tarifas incompatibles con
   igual fecha, una fecha inválida o la ausencia de tarifa aplicable quedan en preguntas. La fecha
   de referencia es la del mensaje/pedido si la trae; si no, la de hoy, y siempre se escribe cuál
   se usó en el encabezado de preguntas (nunca se sustituye en silencio). Si el precio dicho
   difiere del de lista → la línea se excluye por completo de renglones_listos_erp.csv hasta
   aclararla; ambos precios se muestran únicamente en preguntas. Una confirmación de precio
   especial identifica cliente, SKU, importe, moneda y evidencia escrita; si no existe, sigue
   pendiente. **No se captura un precio que no esté en la lista sin que el vendedor lo confirme
   por escrito.**
6. **Revisión de cantidad y unidad:** la unidad dicha cuadra con `unidad_venta = pieza` cuando no se dice unidad o cuando
   es la palabra del empaque que ya está en la `descripcion` (bolsa, botella, garrafón, lata, rollo, hoja): «10 bolsas de
   frijol bayo bolsa 1 kg» = 10 piezas, sin duda. Es `cantidad rara` si: pide en «caja», «paquete», «bulto» o «litros/kilos»
   un SKU que se vende por pieza (si `notas` del catálogo trae la conversión, por ejemplo «24 piezas por caja», se escribe
   en la pregunta); la cantidad es 0, negativa o con decimales en un SKU por pieza. Si el usuario adjunta historial de
   pedidos, también lo es la cantidad mayor a 3 veces su máximo histórico; sin historial, esa revisión no se hace y no se
   menciona.
7. **Fecha de entrega:** si el pedido la trae, se normaliza a `AAAA-MM-DD` y se anota
   explícitamente el día de la semana (un «para el lunes» de WhatsApp puede ser lunes próximo o
   pasado; si es ambiguo contra la fecha de hoy, va a preguntas). Si no la trae, va a preguntas.
   Una fecha pendiente excluye todas las líneas afectadas del CSV hasta aclararla.
8. **Emitir** los dos entregables (formatos abajo).

## Salida 1 — renglones_listos_erp.csv

Solo líneas **100 % empatadas** (SKU + precio + cantidad sin marca de duda). Columnas:

```
cliente_id,sku,cantidad,unidad,precio_unitario,moneda,fecha_entrega,estado
...,...,...,...,...,...,...,OK
```

Cualquier línea con una sola marca de duda **no** entra a este CSV: entra a preguntas. El CSV
nunca se mezcla con «probablemente». El CSV no lleva comentarios ni líneas extra: la fecha de corrida y la fuente van en el encabezado de preguntas_al_vendedor.md. Sin plantilla del
sistema del cliente, es un «CSV genérico listo para copiar y revisar», sin garantizar importación
directa a cualquier ERP. Si el usuario dio su plantilla, se respetan sus columnas; la columna que
no pueda llenarse con datos del pedido, el catálogo o la lista va a preguntas, no se inventa.

## Salida 2 — preguntas_al_vendedor.md

Una pregunta por renglón dudoso, con este formato:

```
## Línea N — «texto original de la línea»
- Tipo de duda: SKU ambiguo | no encontrado | precio distinto | cantidad rara | alternativa condicionada | fecha dudosa | duplicado (una línea puede llevar dos tipos, unidos con " + ")
- Lo que el cliente dijo: ...
- Lo que dice el catálogo/lista: ...
- Pregunta al vendedor: ¿...?
- Evidencia: línea N del pedido + fila(s) del catálogo/lista que chocan
```

Al final, un resumen: `X líneas listas, Y líneas con duda (tipos: ...)` para que el vendedor
vea el costo de responder antes de decidir.

## Cómo se entrega la respuesta

La respuesta trae exactamente dos bloques y nada más: (1) `renglones_listos_erp.csv` dentro de un bloque de código
```csv, listo para copiar; si no hay líneas listas, el bloque trae solo el encabezado; (2) `preguntas_al_vendedor.md` en
Markdown. Sin introducción, sin "aquí tienes", sin resumen repetido fuera del archivo. Dentro del bloque CSV no hay
comentarios, prólogos ni despedidas. Con una entrada completa (pedido + catálogo + lista + fecha de referencia y cliente),
produce directamente ambos bloques finales, sin pedir confirmaciones redundantes; solo las dudas justificadas van a
preguntas. Al pie de preguntas va la cita:
*Basado en una skill de Heroldo Escobedo — heroldo.org*.

## Reglas duras (guardrails)

- **Prohibido inventar SKUs.** Lo que no empata con el catálogo va a preguntas, punto.
- **Prohibido asumir precios.** Si no está en la lista de precios del cliente, no se captura;
  se pregunta. La lista de precios manda sobre lo que dijo el cliente.
- **Prohibido corregir silenciosamente.** Toda diferencia queda escrita en preguntas con la
  evidencia; nada se «arregla» sin dejar rastro.
- **Prohibido inventar reglas fiscales o de facturación.** Esta skill no timbra, no calcula
  impuestos y no decide condiciones comerciales; solo prepara renglones. Lo fiscal lo decide el
  ERP/contador del cliente.
- **Datos:** Los CSV del cliente se tratan como confidenciales: no se suben a servicios de terceros sin permiso escrito del dueño de los datos, y no se pegan contraseñas
  ni llaves del ERP.
- **Empresas reales:** prohibido nombrarlas en ejemplos y demostraciones públicas: los datos de demostración son
  del almacén ficticio **Andén Norte** (ver `ejemplos/`). En una captura privada se conserva el identificador real
  autorizado del comprador y su cliente, sin reemplazarlo por Andén Norte.

## Reglas reusadas del crew logístico (citadas, no copiadas)

- La disciplina de **no ajustar unilateralmente y dejar evidencia escrita** viene del
  `log-inv-n4-erp-wms-reconciler` (Conciliador ERP-WMS) de `logistics-crew-skills`.
- El criterio de **recibo contra lo que se dijo vs lo que existe** se alinea con
  `log-wms-n5-merchandise-checker` (verificador de mercancía).
- La etiqueta de dato **[MEDIDO]/[DECLARADO]/[SUPOSICIÓN]** del kit Andén Norte aplica: lo que
  dijo el cliente es **[DECLARADO]**; lo de la lista de precios es **[MEDIDO]** del CSV; y lo que
  la skill infiere (similitud de texto) es **[SUPOSICIÓN]** escrita como tal.
- No hace falta tener esas skills: la regla ya está escrita aquí.

## Límites

- Es un cerebro de emparejamiento, no una integración: no abre WhatsApp ni el ERP; recibe texto
  y CSV y devuelve CSV y Markdown. La automatización del buzón es trabajo del servicio que la
  monte (la infraestructura de mensajería queda fuera).
- Con catálogos de miles de SKUs, el match por descripción debe correr con umbral de similitud
  y quedar registrado; si la similitud es baja, va a preguntas, no se fuerza.
- Si el pedido viene en imagen sin OCR disponible, se pide el texto o el PDF original: no se
  «lee» una foto a ciegas.
