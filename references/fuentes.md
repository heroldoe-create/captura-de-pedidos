# Referencias y fuentes — captura-de-pedidos

Toda fuente con su URL y la fecha en que se consultó. Ninguna cifra de este paquete sale de una
fuente que no esté aquí o en el scouteo que lo encargó.

## El encargo que la originó

- `creador/producto/SCOUTEO-MICROPROCESOS-2026-10-29.md` (medido el 2026-10-29 con Perplexity,
  perplexica, Indeed MX, OCC y Agensi). De ahí sale la fila «Captura de pedidos de WhatsApp,
  correo o PDF al ERP»: hueco = WhatsApp + catálogo del cliente + ERP mexicano, que los
  genéricos no cubren.

## Competidores genéricos citados en el scouteo (precios publicados, vistos el 2026-10-29)

- Parsio — https://www.parsio.io/ — USD 29–299/mes (scouteo, 2026-10-29).
- Docparser — https://docparser.com/ — USD 39–159/mes (scouteo, 2026-10-29).
- Nanonets — https://nanonets.com/ — USD 100+/mes (scouteo, 2026-10-29).

El punto del scouteo: son extractores genéricos de documentos; este paquete existe para el
cruce conversacional del pedido (WhatsApp, correo o PDF) con el catálogo y los precios que aporta
el comprador, con dos entregables y revisión de excepciones, sin conectar buzones ni sistemas.
Las cifras se citan como las midió el scouteo, no se re-verificaron en esta corrida.

## Formato de skills

- Especificación de Agent Skills (encabezado YAML con `name`, `description`, `license`):
  https://agentskills.io/specification — consultado 2026-10-29.

## Licencia

- Licencia de uso de Heroldo 1.0 (propiedad intelectual de Heroldo Escobedo, código a la vista):
  el texto completo está en `LICENSE` de este paquete y la regla en
  `creador/producto/licencia/POLITICA-LICENCIA-SKILLS.md` (decisión de Heroldo, 2026-10-29);
  plantillas en `creador/producto/licencia/LICENCIA-HEROLDO.md` y
  `creador/producto/licencia/PLANTILLA-README-LICENCIA.md`. La ley que ese LICENSE nombra es
  aparte de las condiciones privadas de uso de Heroldo: [Ley Federal del Derecho de Autor, Cámara
  de Diputados](https://www.diputados.gob.mx/LeyesBiblio/pdf/LFDA.pdf), texto vigente consultado
  2026-10-29, última reforma indicada en ese PDF 14-05-2026.

## Reglas reusadas del crew (citadas en SKILL.md)

- `creador/producto/logistics-crew-skills/skills/log-inv-n4-erp-wms-reconciler/SKILL.md`
  (CC BY 4.0, publicado 2026-10-26) — disciplina de evidencia escrita y prohibición de ajuste
  unilateral.
- `creador/producto/logistics-crew-skills/skills/log-wms-n5-merchandise-checker/SKILL.md`
  (CC BY 4.0) — criterio de verificar contra lo existente, no contra lo dicho.
- `creador/producto/kit-anden-norte/AGENTS.md` — etiquetas de evidencia
  [MEDIDO]/[DECLARADO]/[SUPOSICIÓN] y §4 de seguridad de datos.
