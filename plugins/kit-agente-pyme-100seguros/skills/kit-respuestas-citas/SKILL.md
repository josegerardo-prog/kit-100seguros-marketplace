---
name: kit-respuestas-citas
description: Asistente de respuestas, entrega de puntos y citas del agente 100% Seguros. Ejecutar cuando el agente pegue la respuesta de un prospecto ("me contestó esto", "qué le respondo", "mándale los puntos", "agéndame la cita"), y también como tarea programada diaria EN LA NUBE (~1pm) para el repaso de pipeline y citas. Redacta la contestación canon, genera los 2-3 puntos del giro y agenda en Google Calendar.
---

# Respuestas, puntos y citas

**Antes de todo**: carga la identidad desde la memoria del proyecto (`kit-identidad.md`); si no existe → corre kit-identidad. Aplican las 5 REGLAS INNEGOCIABLES del kit (ver kit-identidad): toda lectura de Airtable filtrada por Agente = su nombre completo; jamás toques ni muestres leads de otros.

Base `appnfv9ZIaqzKfRwg` · Contactos `tblYVlPNaTs3CQfDw` (Negocio `fldpqr5RDtNWUn2Jc`, Teléfono `fldBbVAnaZKgwvu5H`, Agente `fldY87ilp95V3Mip3`, Fecha `fldUprZ41CLEPNLZz`, Giro `fldQ0NnjyOxQ5ThWS`, Estatus `fldLJp1Ns7qEScBsN`, Tema `fldraERshpAO7rRuj` (el campo de notas del kit)) · Actividad Diaria `tblYpC5aHIHsAnmZF`.

## MODO A — A demanda: el agente pega una respuesta

1. Identifica el lead (por nombre del negocio o teléfono) entre SUS registros. Actualiza Estatus → "Respondio" (así, sin acento — es el nombre exacto de la opción en la base) y añade a Notas la respuesta recibida con fecha (nunca borres notas previas).
2. Clasifica la respuesta y redacta la contestación (misma voz canon: de usted, cálido, llano, sin "seguros"/"asesor" hasta que haya cita, sin enlaces, 30-60 palabras):
   - **"Sí, mándame los puntos"** → GENERADOR DE PUNTOS: lee el mapa de dolor del kit (`${CLAUDE_PLUGIN_ROOT}/skills/kit-prospeccion-pyme/references/mapa-dolor-giros.md`), toma el giro del lead y lo raspado (señal en Notas), y redacta **2-3 puntos personalizados a SU negocio** (riesgo concreto + por qué le pega + pregunta que haga pensar). Cierra ofreciendo revisarlos juntos: "Si quiere, en 20 minutos se los aterrizo con números de su negocio — ¿le acomoda esta semana?". Marca en Notas `[puntos:FECHA]`.
   - **Interés / pregunta** → contesta la duda en corto por la ruta diagnóstico (entender su negocio antes de hablar de soluciones) y propón la mini-cita de 20 minutos.
   - **"Ahorita no" / tibio** → respuesta digna y breve, sin insistir: agradece, deja la puerta abierta ("aquí ando cuando le sirva") y marca Notas `[tibio:FECHA]`. El día 7 lo retoma la skill de seguimiento.
   - **"No me interesa / no moleste"** → Estatus → "No interesado", respuesta de una línea agradeciendo, y NUNCA volver a contactarlo.
3. **Si acepta cita**: propón 2-3 horarios leyendo la disponibilidad real en su Google Calendar; al confirmar el prospecto, crea el evento (título "Cita — <Negocio>", 30 min, con teléfono y giro en la descripción) y actualiza Estatus → "Cita". PIDE confirmación del agente antes de crear el evento.
4. Telemetría: suma 1 a la fila de Actividad Diaria del día del agente (Respuestas `fldkeFDgRAXUzw3TH`; Puntos entregados `fldVajxqsC2q3p3qP` si aplicó; Citas `fldmAaFuvtX422dWR` si aplicó). Si no existe fila del día, créala con su nombre. Cuando el agente reporte un cierre ("ya es cliente"), Estatus → "Cliente" y suma en Cierres `fldes4pyBUce4pjeB` — la administración de su cartera a partir de ahí vive en el portal de Click Seguros, no en el kit. Si necesita armar la propuesta formal de una cotización, dirígelo a kit-propuesta-pdf.

## MODO B — Programado en la nube (repaso diario ~1pm)

Corre TODO filtrado por Agente = su nombre completo:
1. **WA-Listos rezagados**: los de hace >48h sin cambiar a Contactado → lista corta de recordatorio ("tienes N pendientes de enviar").
2. **"Respondio" sin siguiente paso**: Estatus "Respondio" sin nota `[puntos:]` ni cita en >24h → recuérdale con el borrador de contestación listo.
3. **Citas de hoy y mañana**: léelas de su Google Calendar; prepárale por cada una un mini-brief (negocio, giro, señal, puntos entregados, qué preguntar en ruta diagnóstico — el cross-sell a vida/GMM solo se menciona EN la cita).
4. **Hilos de Gmail**: si algún prospecto le escribió por correo (búsqueda nativa por los emails de SUS leads), resúmelo y bórrale la duda con un borrador de respuesta. El kit NO manda correo en frío; Gmail es solo para responder hilos existentes.
5. Entrega un resumen único y accionable (máx 15 líneas). Si no hay nada, dilo en una línea y termina.

Esta es la ÚNICA tarea del kit que corre en la nube (necesita conexión MCP estable); todas las demás corren en la computadora del agente.
