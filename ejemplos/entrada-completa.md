# Entrada completa — captura-de-pedidos (lista para pegar)

Todo el caso —incluidos Rosario y La Esquina del Norte— es ficticio, del almacén Andén Norte.
Pega el bloque de abajo en una conversación donde ya conectaste la skill (ver `README.md`,
«Uso en 3 pasos»): con esta entrada completa la skill produce directamente ambos bloques
finales, sin pedir confirmaciones redundantes; solo las dudas justificadas van a preguntas.

```
Captura este pedido con este catálogo y esta lista de precios; hoy es 2026-10-29, cliente 201.
Dame exactamente los dos entregables: `renglones_listos_erp.csv` en un bloque de código csv
listo para copiar, y `preguntas_al_vendedor.md` en Markdown.

PEDIDO (pedido_whatsapp.txt):
Buenas, soy Rosario de la tienda La Esquina del Norte (cliente 201).

Paseme surtido por favor:

- 10 bolsas de frijol bayo 1 kg
- 24 atun lomito agua 140 grs
- 3 cajas de papel kraft para envoltura
- 5 litros de aceite 5L (si no hay, 10 de 1 litro)
- 12 refresco cola 600 ml a 18 pesos

Lo necesito para el sabado 31 de octubre. Gracias!

CATÁLOGO (catalogo_anden_norte.csv):
sku,descripcion,unidad_venta,codigo_barras,activo,notas
FRB-001,frijol bayo bolsa 1 kg,pieza,7501000000011,si,
ATN-002,atun lomito en agua 140 g,pieza,7501000000028,si,
PKR-010,papel kraft rollo 60 cm,pieza,7501000000103,si,venta por rollo
PKR-011,papel kraft hoja 1 m,pieza,7501000000110,si,venta por hoja
ACE-003,aceite vegetal botella 1 L,pieza,7501000000035,si,
ACE-004,aceite vegetal garrafon 5 L,pieza,7501000000042,si,
REF-006,refresco cola 600 ml,pieza,7501000000066,si,empaque 24 piezas por caja

LISTA DE PRECIOS DEL CLIENTE 201 (lista_precios_cliente_201.csv):
cliente_id,sku,precio,moneda,vigente_desde
201,FRB-001,52.00,MXN,2026-09-01
201,ATN-002,24.50,MXN,2026-09-01
201,PKR-010,89.00,MXN,2026-09-01
201,PKR-011,12.00,MXN,2026-09-01
201,ACE-003,41.00,MXN,2026-09-01
201,ACE-004,189.00,MXN,2026-09-01
201,REF-006,19.00,MXN,2026-09-01
```

Lo que debe salir está en `salida_esperada/`: **2 renglones listos** (frijol bayo ×10, atún
×24) y **3 preguntas** (papel kraft ambiguo y en cajas, aceite con cantidad rara y alternativa,
refresco con precio distinto).
