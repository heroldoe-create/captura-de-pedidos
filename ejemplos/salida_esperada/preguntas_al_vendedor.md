# Preguntas al vendedor — pedido cliente 201
Fecha de referencia: 2026-10-29 (dada por el usuario; el mensaje no trae fecha).
Fuente: pedido_whatsapp.txt · Catálogo: catalogo_anden_norte.csv · Lista: lista_precios_cliente_201.csv
Formato genérico: ajusta los nombres de columna a tu ERP.

## Línea 3 — «3 cajas de papel kraft para envoltura»
- Tipo de duda: SKU ambiguo + cantidad rara
- Lo que el cliente dijo: «papel kraft para envoltura», 3 «cajas».
- Lo que dice el catálogo: dos candidatos razonables:
  - PKR-010 papel kraft rollo 60 cm — $89.00 (venta por rollo)
  - PKR-011 papel kraft hoja 1 m — $12.00 (venta por hoja)
- Pregunta al vendedor: ¿Cuál de los dos papel kraft quiere la cliente, y «3 cajas» a cuántos
  rollos/hojas equivale según el empaque que le surtan?
- Evidencia: línea 3 del pedido; filas PKR-010 y PKR-011 del catálogo.

## Línea 4 — «5 litros de aceite 5L (si no hay, 10 de 1 litro)»
- Tipo de duda: cantidad rara + alternativa condicionada
- Lo que el cliente dijo: «5 litros» de aceite de 5 L; si no hay, «10 de 1 litro».
- Lo que dice el catálogo: ACE-004 aceite vegetal garrafon 5 L ($189.00 de lista) y ACE-003 aceite vegetal botella 1 L
  ($41.00 de lista); ambos activos y se venden por pieza.
- Pregunta al vendedor: «5 litros» de garrafón de 5 L es 1 garrafón, pero la alternativa (10 botellas de 1 L) suma
  10 litros. ¿Cuántos garrafones ACE-004 quiere la cliente: 1, 2 o 5? Y si no hay existencia, ¿cuántas botellas ACE-003
  se surten en su lugar?
- Evidencia: línea 4 del pedido; filas ACE-004 y ACE-003 del catálogo y de la lista del cliente 201.

## Línea 5 — «12 refresco cola 600 ml a 18 pesos»
- Tipo de duda: precio distinto
- Lo que el cliente dijo: 12 piezas de REF-006 a $18.00.
- Lo que dice la lista de precios: REF-006 para el cliente 201 está a $19.00 MXN vigente desde
  2026-09-01. La lista manda hasta que el vendedor confirme otro precio por escrito.
- Pregunta al vendedor: La lista dice $19.00 y la cliente pide a $18.00. ¿Se le honra el precio
  de $18.00 o se le avisa del precio de lista?
- Evidencia: línea 5 del pedido; fila REF-006 de lista_precios_cliente_201.csv.

---

Resumen: 2 líneas listas (FRB-001 ×10, ATN-002 ×24), 3 líneas con duda (1 SKU ambiguo
+ cantidad rara, 1 cantidad rara con alternativa condicionada, 1 precio distinto). Fecha de
entrega normalizada: 2026-10-31 (sábado, sin ambigüedad contra la fecha del pedido).

*Basado en una skill de Heroldo Escobedo — heroldo.org*
