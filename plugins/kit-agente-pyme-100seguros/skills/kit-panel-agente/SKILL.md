---
name: kit-panel-agente
description: Panel WA-Listo del agente 100% Seguros, EN VIVO. Ejecutar cuando el agente diga "mi panel", "mis WhatsApps pendientes", "refresca mi panel", o al final de las corridas de prospección y seguimiento EN SU COMPUTADORA. Crea UNA VEZ el artifact persistente "panel-agente" en la galería de Cowork del agente y lo ACTUALIZA en su lugar (nunca archivos sueltos ni duplicados). El panel lee Airtable en tiempo real al abrirse — cola de WhatsApps, seguimientos y números, solo con SUS registros.
---

# Panel WA-Listo del agente

El centro de operación diario del agente: abrirlo, enviar sus WhatsApps, marcar avances. Es un artifact local en SU computadora, generado exclusivamente con SUS registros.

**Antes de todo**: carga la identidad desde la memoria del proyecto (`kit-identidad.md`); si no existe → corre kit-identidad. Aplican las 5 REGLAS INNEGOCIABLES del kit (ver kit-identidad). **AISLAMIENTO ESTRICTO**: cada consulta de este panel lleva el filtro Agente `fldY87ilp95V3Mip3` = nombre completo del agente. Ningún dato de otro agente puede entrar al HTML, ni en totales ni en listas. El "Radar de la Red" (vista global) es exclusivo de Gerardo y NO existe en este kit.

## EL PANEL ES EN VIVO: lee Airtable al abrirse, no lleva datos horneados

El HTML NO contiene los leads incrustados: la propia página consulta Airtable EN VIVO cada vez que el agente la abre o pulsa refrescar, usando el conector de Airtable del agente. Así el panel siempre está al día sin esperar la corrida. Mecánica obligatoria (idéntica a los paneles del promotor):

1. **Bloque de metadatos al inicio del HTML** (antes de `<html>`), declarando las herramientas MCP que la página usará — con los NOMBRES EXACTOS de las herramientas de Airtable disponibles en TU sesión al crear el panel (incluyen el prefijo del conector del agente, p. ej. `mcp__<id-del-conector>__list_records_for_table`):
```
<script type="application/json" id="cowork-artifact-meta">
{ "name": "Panel del Agente", "schemaVersion": 1,
  "description": "Panel en vivo del agente: cola WA-Listo, seguimientos y números, leído en tiempo real de Airtable (solo sus registros).",
  "mcpTools": ["<tool list_records_for_table>", "<tool update_records_for_table>", "<tool create_records_for_table>"],
  "mcpServerNames": ["Airtable", "Airtable", "Airtable"] }
</script>
```
2. **Llamadas en vivo desde el JS de la página** con `window.cowork.callMcpTool(nombreDeTool, argumentos)`; parsea la respuesta de `r.structuredContent` o, si no viene, de `JSON.parse(r.content[0].text)`, y toma los campos de `rec.cellValuesByFieldId || rec.fields`.
3. **Cablea como constantes en el HTML**: baseId `appnfv9ZIaqzKfRwg`, los tableIds y fldIds de abajo, y el NOMBRE COMPLETO del agente (de su identidad guardada). CADA lectura de Contactos y de Actividad Diaria lleva el filtro por Agente en la llamada: `filters: {"operands":[{"operator":"=","operands":["<fld Agente>","<nombre completo>"]}]}` (Agente es campo de texto: la igualdad con string funciona). El filtrado por Estatus se hace del lado del cliente sobre los nombres de opción ("WA-Listo", "Contactado", "Respondio" sin acento, etc.) — así la página no depende de choice IDs.
4. **Acciones en vivo desde el panel**: botón "✅ Ya lo envié" en cada tarjeta WA-Listo → `update_records_for_table` cambiando Estatus a "Contactado" (el valor se escribe como texto con el nombre exacto de la opción), y además suma 1 a "WA enviados" en la fila de Actividad Diaria del día del agente (búscala con filtro Agente+Fecha; si no existe, créala con `create_records_for_table`). Botón "🚫 Sin WhatsApp" → Estatus "Sin WhatsApp". Muestra confirmación (toast) y actualiza la vista sin recargar.
5. Maneja errores con mensaje amable ("no pude conectar con Airtable, revisa tu conector") — nunca en silencio.

## Datos (todos de base `appnfv9ZIaqzKfRwg`, todos filtrados por su Agente)

1. **Cola WA-Listo**: Contactos `tblYVlPNaTs3CQfDw` con Estatus "WA-Listo" — negocio, teléfono, giro, zona (campo Ciudad), fecha, y el mensaje redactado (desde Tema `fldraERshpAO7rRuj`, el campo de notas del kit).
2. **Seguimientos del día**: filas "Contactado" con `[seg7:]` de hoy en Tema — con su mensaje de seguimiento.
3. **Pipeline vivo**: conteos por Estatus (WA-Listo, Contactado, "Respondio" — así, sin acento, nombre exacto de la opción —, Cita, Cliente, No interesado) y lista corta de "Respondio" pendientes de siguiente paso.
4. **Sus números**: de Actividad Diaria `tblYpC5aHIHsAnmZF` (filtrado por su Agente), últimos 14 días: WA enviados, respuestas, citas, cierres, gasto Apify; y su avance de hoy contra su cupo de rampa.

## Generar el panel COMO ARTIFACT PERSISTENTE (no como archivo suelto)

El panel es un ARTIFACT de Cowork en la computadora del agente: vive en su galería/barra lateral de artifacts, se abre con un clic sin buscar archivos, y SIEMPRE se actualiza EN SU LUGAR. PROHIBIDO entregar el panel como archivo .html suelto o crear uno nuevo cada día.

1. Construye el HTML autocontenido (CSS/JS inline, sin recursos externos, con las secciones de abajo).
2. **PRIMERA VEZ**: crea el artifact con la herramienta de artifacts del entorno local (`create_artifact`), título "Panel del Agente" (nombre estable `panel-agente`), y GUARDA su identificador en la memoria local del kit (`kit-identidad.md`) para reutilizarlo siempre.
3. **TODAS LAS CORRIDAS SIGUIENTES**: actualiza ESE MISMO artifact (`update_artifact` con el identificador guardado). Jamás crees un segundo panel; si por error existieran dos, actualiza el original y avisa al agente para borrar el duplicado.
4. Si las herramientas de artifacts no estuvieran disponibles en la corrida (p. ej. no es una sesión local de escritorio), entrega el mismo HTML como archivo, avisa que el artifact se refresca en la próxima corrida local, y NO borres el identificador guardado.

Secciones en este orden:
1. **Header**: nombre del agente, fecha, cupo del día y avance (enviados/cupo), racha de días cumplidos.
2. **📲 Por enviar hoy**: tarjetas de la cola WA-Listo — negocio, giro, botón "Abrir WhatsApp" (link `https://wa.me/52<10dígitos>?text=<mensaje URL-encoded>`) y el mensaje visible con botón copiar. Orden: más viejos primero. Los >48h marcados en ámbar.
3. **🔁 Seguimientos día 7**: mismas tarjetas para los seguimientos de hoy.
4. **💬 Respondieron**: quiénes esperan siguiente paso (para que le pida a Claude la contestación/los puntos con kit-respuestas-citas).
5. **📈 Mis números**: tira de 14 días (enviados, respuestas, citas, cierres) + tasa de respuesta.

Estética: limpia, cálida, dorado #c98a2d como acento (identidad 100% Seguros), legible en móvil y escritorio.

## Marcar avances

Cuando el agente avise (por el chat) "ya envié los de hoy" / "el de <negocio> ya" → actualiza Estatus "WA-Listo" → "Contactado" en esas filas (y suma WA enviados `fldqelfBucfyjGxGR` en la fila de Actividad Diaria del día, creándola con su nombre si falta). Si avisa que un número no tiene WhatsApp → Estatus "Sin WhatsApp" + nota `[sin-wa:FECHA]`, sin contarlo como enviado. Luego regenera el panel. Nunca marques enviado sin que el agente lo diga.
