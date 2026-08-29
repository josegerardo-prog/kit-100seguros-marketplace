---
name: kit-prospeccion-pyme
description: Corrida diaria de prospección PyME del agente 100% Seguros. Ejecutar cuando el agente diga "corre mi prospección", "dame mis WhatsApps de hoy", "prospección de hoy", o desde su tarea programada diaria EN SU COMPUTADORA (L-V ~7am). Descubre PyMEs con Apify en su territorio, deduplica contra la red y deja su lote WA-Listo con mensajes redactados.
---

# Prospección PyME diaria (motor del agente)

Genera el lote diario de WhatsApps listos para enviar. El WhatsApp SIEMPRE lo manda el agente a mano; tu trabajo termina en dejar cada lead registrado con su mensaje redactado. Ejecuta de corrido, sin pedir confirmaciones. NO uses navegador. NO generes CSV. NO mandes correos.

**Antes de todo**: carga la identidad desde la memoria del proyecto (`kit-identidad.md`). Si no existe → corre primero la skill kit-identidad y detente aquí. Aplican las 5 REGLAS INNEGOCIABLES del kit (ver kit-identidad).

## PASO 0 — Cupo del día (rampa anti-baneo)

Calcula semanas desde la fecha de arranque guardada: semanas 1-2 → cupo 8; semanas 3-4 → cupo 14; después → su meta diaria (de su identidad guardada, típico 20). Antes de generar nuevos, revisa cuántos WA-Listo SUYOS siguen sin enviar (ver PASO 5): si tiene ≥ cupo pendientes, NO generes más — recuérdale enviarlos y termina con telemetría en cero preparados.

## PASO 1 — Rotar giro y zona (dentro de SU territorio)

Usa SOLO los giros y zonas de su identidad guardada. Rotación default: giro índice = día_del_año mod (número de sus giros); zona ídem con sus zonas. Si su configuración dice "TODOS", usa la lista maestra de giros: ferretería, refaccionaria, taller mecánico, mueblería, papelería, farmacia, restaurante, cafetería, abarrotes mayoreo, distribuidora, panadería, tortillería, carpintería, vidriería, herrería, imprenta, lavandería, estética y spa, veterinaria, consultorio dental, gimnasio, escuela privada / kínder, constructora, bodega / almacén, agencia de autos, vinatería, florería, mercería, purificadora de agua. Prioriza giros con local, inventario o vehículos.

## PASO 2 — Descubrir (Apify)

`call-actor` **compass/crawler-google-places** (async, poll hasta SUCCEEDED, luego dataset). Input: `{"searchStringsArray":["<giro>"],"locationQuery":"<zona>","maxCrawledPlacesPerSearch":30,"language":"es","countryCode":"mx","scrapePlaceDetailPage":true,"maxReviews":3,"reviewsSort":"mostRelevant","scrapeContacts":true}`. Guarda por negocio: nombre, giro, ciudad, TELÉFONO (imprescindible), email si hay, calificación, # reseñas, reseñas (señal humana), señales de sucursales/antigüedad. **TOPE APIFY: ~$1.00 USD/corrida** (arranque en rampa: ~$0.50). Registra el gasto real para la telemetría.

## PASO 3 — Calificar

PyMEs LOCALES con teléfono publicado. DESCARTA: cadenas/franquicias nacionales (OXXO, Coppel, Elektra, Farmacias Guadalajara/del Ahorro, Autozone, etc.), negocios cerrados temporal o permanentemente, y teléfonos evidentemente inválidos. Un registro por negocio (mismo nombre+ciudad = uno).

## PASO 4 — DEDUP (sagrado): contra TODA la tabla Contactos

Todos — la promotoría y todos los agentes — escriben en la misma tabla, así que el dedup es un solo cruce y siempre está al día. Por cada candidato, `list_records_for_table` sobre Contactos `tblYVlPNaTs3CQfDw` (NUNCA `search_records`) con filtro exacto SIN filtrar por Agente ni Segmento (el cruce es contra TODO): Telefono `fldBbVAnaZKgwvu5H` contains (últimos 10 dígitos) — y si trae email, Email `fldNwk2c7aXsYiuDX` igualdad. Si existe, con cualquier estatus, segmento o dueño: descártalo en silencio — jamás muestres ni comentes el registro existente ni de quién es (esta consulta solo responde "existe / libre"). Quédate con NUEVOS hasta llenar el cupo. Si el giro/zona del día no alcanza el cupo, repite PASO 2 con el siguiente giro de su lista (máx 2 giros extra por corrida, respetando el tope Apify); si aun así no alcanza, entrega lo que haya y anótalo en telemetría.

## PASO 5 — Registrar el lote WA-Listo (la reserva del lead)

UNA llamada `create_records_for_table` (typecast=true), una fila por lead:
- Negocio `fldpqr5RDtNWUn2Jc` · Teléfono `fldBbVAnaZKgwvu5H` (formato +52 y 10 dígitos) · Email `fldNwk2c7aXsYiuDX` (si hay) · **Agente `fldY87ilp95V3Mip3` = nombre completo del agente** · Fecha `fldUprZ41CLEPNLZz` = hoy YYYY-MM-DD · Giro `fldQ0NnjyOxQ5ThWS` · Ciudad `fldocduZLpjrJ85xp` (la zona) · **Estatus `fldLJp1Ns7qEScBsN` = "WA-Listo"** · Segmento `fldkVYClwWfHGRndw` = "PyME" · Canal `fldAGeRoHff3QxgGv` = "WhatsApp" · Source `fld6NbrHA8RL8VbOC` = "Kit agente" · Nombre `fldvwCAWV6VxTouUl` (dueño, si se sabe) · Tema `fldraERshpAO7rRuj` (el campo de notas del kit) = señal usada + el mensaje redactado completo.

Escribir la fila WA-Listo RESERVA el lead ante toda la red: por eso se registra ANTES de enviar, y lo antes posible — nunca dejes pasar tiempo entre el dedup y el registro (si la corrida se interrumpe y la retomas, repite el dedup antes de registrar). Cuando el agente avise que envió, cambia Estatus a "Contactado" (nunca borres la nota). Si el agente reporta que un número NO tiene WhatsApp: Estatus → "Sin WhatsApp" + nota `[sin-wa:FECHA]` (no cuenta como enviado; el lead queda reservado y bloqueado para toda la red) y repón ese lugar del cupo en la siguiente corrida.

## PASO 6 — Redactar cada WhatsApp (canon del kit)

Reglas duras: 45–60 palabras, de USTED, cálido y llano, tono de persona (no de compañía), SIN la palabra "seguros" ni "asesor" ni nombre de empresa, SIN enlaces, sin mayúsculas de grito. Estructura:
1. Apertura = UNA señal real y específica del negocio (de reseñas, antigüedad, sucursales). No inventes.
2. Presentación humana: "Soy <nombre de pila del agente>, aquí en <su ciudad>; me dedico a ayudar a dueños de negocio a proteger lo que les ha costado construir."
3. CTA de interés con el gancho de los puntos: "Suelo revisar 2-3 puntos concretos en <giro>s como el suyo y casi siempre sale algo útil. ¿Le late que se los comparta? Sin compromiso."
4. Salida mínima: "Si no es momento, sin problema."

Consulta `references/mapa-dolor-giros.md` para conocer el dolor del giro y afinar la señal — pero los 2-3 puntos NO se mandan en el primer mensaje: se prometen; se entregan cuando responda (skill kit-respuestas-citas). Varía apertura y fraseo entre mensajes; nunca dos mensajes idénticos.

## PASO 7 — Telemetría (obligatoria)

UNA fila en **Actividad Diaria** `tblYpC5aHIHsAnmZF`: Registro `fldrKPLjS3qQGfKPK` = "<agente> — <fecha> — prospección" · Fecha `fld7DdDOp1VHff0dr` · **Agente `fldS7eQUbSSjAzTHw` = nombre completo** · Giro `fldr0Kl6xBeGgneO5` · Zona `fldp2uNIanDR9YlKo` · WA preparados `fldtPM6uzcNgt0SGJ` = # del lote · Gasto Apify `fldPbFVmnMtBVR1XH` = USD real · Notas `fldM39AcXxCaQIjgO` = incidencias (giro saturado, cupo no alcanzado, etc.).

## PASO 8 — Entregar y refrescar panel

Resume en 3 líneas: giro/zona, # preparados, # descartados por dedup. Luego corre la skill kit-panel-agente para refrescar su panel con el lote nuevo (links wa.me y mensajes listos para copiar). Recuérdale: enviar a mano, espaciado en el día (no ráfagas), y avisarte quién respondió.
