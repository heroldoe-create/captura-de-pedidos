# Captura de pedidos — de WhatsApp, correo o PDF a renglones del ERP

Skill que lee un pedido libre (texto de WhatsApp, cuerpo de correo o PDF), extrae cliente,
SKUs, cantidades, precios y fecha de entrega, lo empata con el **catálogo** y la **lista de
precios del cliente** (CSV) y produce dos archivos:

1. `renglones_listos_erp.csv` — solo las líneas que empataron 100 %, listas para copiar al formato de tu ERP.
2. `preguntas_al_vendedor.md` — todo lo dudoso (SKU ambiguo, precio distinto, cantidad rara,
   fecha dudosa, duplicado) con la pregunta exacta y su evidencia.

**La regla central: no inventa SKUs.** Lo que no empata con el catálogo va a preguntas; lo que
no está en la lista de precios no se captura hasta que el vendedor lo confirme. La skill cruza,
marca diferencias y arma evidencia: no sustituye al criterio comercial del vendedor ni a las
decisiones fiscales del ERP/contador del cliente.

## Estructura del paquete

```
captura-de-pedidos/
├── LICENSE                              Licencia de uso de Heroldo 1.0 (texto oficial)
├── README.md                            este archivo
├── skills/captura-de-pedidos/SKILL.md   el cerebro (Agent Skill)
├── references/fuentes.md                fuentes con URL y fecha de consulta
└── ejemplos/                            caso completo con datos ficticios de Andén Norte
    ├── pedido_whatsapp.txt              entrada: pedido libre
    ├── catalogo_anden_norte.csv         catálogo del vendedor (ficticio)
    ├── lista_precios_cliente_201.csv    lista de precios del cliente (ficticia)
    ├── entrada-completa.md              todo lo anterior en un solo bloque listo para pegar
    └── salida_esperada/                 lo que debe producir la skill
        ├── renglones_listos_erp.csv
        └── preguntas_al_vendedor.md
```

## Uso en 3 pasos (sin instalar nada)

1. **Conecta la skill:** desde el conector de Agensi en claude.ai, o sube `captura-de-pedidos.zip` como skill, o pega el
   contenido de `SKILL.md` al inicio del chat.
2. **Adjunta tres cosas:** el pedido (texto de WhatsApp o correo pegado, o el PDF), tu catálogo y la lista de precios del
   cliente (CSV o Excel con las columnas que ya uses; si falta una imprescindible, la skill la pide y no la inventa).
   Escribe delante la fecha de referencia y el cliente (por ejemplo «hoy es 2026-10-29, cliente 201»). El formato de tu
   sistema de pedidos es opcional: sirven los archivos que ya tengas, sin instalar ni configurar nada.
3. **Pide los dos entregables por su nombre**, con este mensaje listo para copiar:

   «Captura este pedido con el catálogo y la lista de precios adjuntos; hoy es 2026-10-29, cliente 201. Dame exactamente
   los dos entregables: `renglones_listos_erp.csv` en un bloque de código csv listo para copiar, y
   `preguntas_al_vendedor.md` en Markdown. Si falta una columna imprescindible, dime cuál y no la inventes.»

   Recibes el CSV y las preguntas, listos para copiar. El asistente hace el cruce; tú no sigues los ocho pasos internos
   del `SKILL.md` ni conviertes archivos con scripts.

Sin terminal, sin scripts, sin API ni configuración.

En los pasos de arriba `SKILL.md`, `references/` y `ejemplos/` están escritos como quedan dentro del ZIP; en el
repositorio el SKILL.md está en `skills/captura-de-pedidos/`.

## Prueba sin tus datos

Pega este texto literal al inicio del chat junto con los tres archivos de `ejemplos/` (`pedido_whatsapp.txt`,
`catalogo_anden_norte.csv`, `lista_precios_cliente_201.csv`): «Captura este pedido con este catálogo y esta lista de
precios; hoy es 2026-10-29». Todo el caso —incluidos Rosario y La Esquina del Norte— es ficticio, del almacén Andén
Norte.

## Comprobar el resultado

1. Lee `ejemplos/pedido_whatsapp.txt` (5 líneas de pedido).
2. Pega `ejemplos/entrada-completa.md` en una conversación con la skill conectada.
3. Compara tu resultado contra `ejemplos/salida_esperada/`: deben quedar **2 renglones listos**
   (frijol bayo ×10, atún ×24) y **3 preguntas** (papel kraft ambiguo y en cajas,
   aceite con cantidad rara y alternativa, refresco con precio distinto).

## Origen

Segunda ola del scouteo de micro-procesos del 2026-10-29
(`creador/producto/SCOUTEO-MICROPROCESOS-2026-10-29.md`, fila «Captura de pedidos de WhatsApp,
correo o PDF al ERP»): el trabajo que hoy hace un capturista. Lo que entrega este paquete es
comprobable: cruce conversacional del pedido contra el catálogo y los precios que aporta el
comprador, dos entregables (renglones para el ERP y preguntas con evidencia) y revisión de
excepciones, sin conectar buzones ni sistemas. Cuesta USD 20 por ese cruce y esas excepciones;
no promete ahorrar un sueldo ni vender más.
Reusa criterios del crew logístico (CC BY 4.0): disciplina de evidencia del Conciliador
ERP-WMS y el criterio de verificación del verificador de mercancía; ambos citados en el SKILL.md.

---

## Licencia

**Propiedad intelectual de Heroldo Escobedo**, con el código a la vista ([Licencia de uso de Heroldo 1.0](LICENSE)):

- **Uso personal: gratis.** Léelo, pruébalo y adáptalo para aprender o en tus propios datos.
- **Uso en una empresa o con fines de lucro: solo con compra.** Cómpralo en [Agensi](https://agensi.io/skills/captura-de-pedidos)
  o escribe a heroldo@lolo-enterprises.com. La compra cubre a tu empresa.
- **Siempre se cita la fuente:** «Basado en una skill de Heroldo Escobedo — heroldo.org».
- No se revende ni se vuelve a publicar.

> **In English:** Source-available, not open source. Free for personal use. Business use requires purchase on
> [Agensi](https://agensi.io/skills/captura-de-pedidos). Attribution to Heroldo Escobedo — heroldo.org is always required.
