---
name: kit-propuesta-pdf
description: Generador de propuestas en PDF del agente 100% Seguros. Ejecutar cuando el agente diga "ármame la propuesta", "hazme el PDF de la cotización", "tengo estas cotizaciones", "propuesta para tal negocio", o pegue/suba cotizaciones de aseguradoras. Convierte cotizaciones técnicas en una propuesta de una página, limpia y sin tecnicismos, que el cliente entiende y decide rápido.
---

# Propuesta en PDF (de cotización a "sí, va")

Convierte las cotizaciones que el agente trae (texto pegado, captura de pantalla o PDF de la aseguradora) en UNA propuesta profesional que el dueño de negocio entiende en 2 minutos y puede decidir sin llamar a nadie. La velocidad es parte del cierre: la propuesta debe salir en la misma sesión, lista para mandar por WhatsApp.

**Antes de todo**: carga la identidad desde la memoria del proyecto (`kit-identidad.md`); si no existe → corre kit-identidad. Aplican las 5 REGLAS INNEGOCIABLES del kit (ver kit-identidad).

## PASO 1 — Recibir las cotizaciones

Acepta lo que traiga: texto pegado, capturas, PDFs de aseguradoras. Extrae por cada opción: aseguradora, ramo, coberturas con sumas aseguradas, deducibles/coaseguros, prima total y formas de pago. **REGLA DE ORO: jamás inventes ni completes una cobertura, suma o precio que la cotización no diga.** Si falta un dato clave (prima, suma principal, nombre del cliente/negocio), pregúntalo en UNA sola tanda antes de armar. Máximo 3 opciones en una propuesta; si trae más, ayúdale a elegir las 2-3 mejores y pregúntale cuál quiere marcar como recomendada.

## PASO 2 — Traducir a lenguaje humano (cero tecnicismos)

Cada cobertura se escribe como lo que el cliente VIVE, no como se llama en la póliza. Traducciones obligadas (mismo espíritu para todo lo demás):
- "Responsabilidad civil" → "Si tu negocio le causa un daño a alguien más (un cliente, un vecino), esto responde por ti"
- "Deducible de $X" → "En caso de siniestro, tú pones los primeros $X y la aseguradora el resto"
- "Pérdida de beneficios / interrupción" → "Si tienes que cerrar por un siniestro, esto paga la renta y la nómina mientras reabres"
- "Contenidos" → "Todo lo que hay dentro: mercancía, equipo, mobiliario"
- "Bienes en custodia" → "Lo que tus clientes dejan en tus manos"
- Sumas y primas SIEMPRE con formato claro ($1,500,000 MXN) y la prima también en mensualidad si hay pago fraccionado.
Prohibido en el PDF: siglas (RC, PB, TRM), jerga de póliza, letras chiquitas fingidas. Todo lo que no se pueda decir simple, se dice en la cita, no en la propuesta.

## PASO 3 — Armar el PDF con la PLANTILLA OFICIAL (1 página ideal, 2 MÁXIMO)

**Usa SIEMPRE la plantilla del kit**: `${CLAUDE_PLUGIN_ROOT}/skills/kit-propuesta-pdf/references/plantilla-propuesta.html`. Copia el archivo, sustituye los `{{PLACEHOLDERS}}` con los datos reales y borra los bloques que no apliquen — NO rediseñes desde cero ni cambies tipografías, colores o estructura: la plantilla ES la marca de la red (acento dorado #c98a2d, carta, aire, cero saturación). Contenido por sección:
1. **Encabezado**: negocio del cliente, fecha, y los datos DEL AGENTE (su nombre completo y su WhatsApp, tomados de su identidad guardada). **La propuesta es del agente y de nadie más**: jamás pongas datos de Gerardo ni de la promotoría, y jamás menciones "100% Seguros" ni "Red 100% Seguros" en el PDF — el cliente contrata con SU agente.
2. **"Lo que está en juego"** (2-3 líneas): personalizado al giro usando el mapa de dolor del kit (`${CLAUDE_PLUGIN_ROOT}/skills/kit-prospeccion-pyme/references/mapa-dolor-giros.md`) y lo que se sepa del negocio. Tono de cuidado, no de miedo.
3. **Las opciones**: clase `una`/`dos`/`tres` según cuántas; la recomendada lleva la clase `recomendada` y el badge. Por opción: aseguradora, 4-6 líneas de "Qué te cubre" en lenguaje humano, precio anual y mensual. Con una sola cotización va sola y destacada — no rellenes con opciones inventadas.
4. **"Así de simple"** (3 pasos, ya vienen en la plantilla).
5. **Cierre**: WhatsApp del agente + vigencia de la cotización + la línea legal del pie (ya viene).

**Disciplina de páginas — regla dura**: lo ideal es UNA página; si el detalle amerita explicación, se activa la página 2 (bloque comentado en la plantilla: tabla "El detalle, en palabras simples" + "Preguntas que seguro trae"). **JAMÁS tres páginas**: si no cabe en 2, recorta el detalle a lo esencial — una propuesta larga no se lee y mata el no-brainer.

**Render y control de calidad (obligatorio antes de entregar)**:
1. Convierte el HTML a PDF real (si está disponible la skill `pdf` de Anthropic, léela y úsala; Chromium headless `--print-to-pdf` también sirve). Carta, sin márgenes extra del navegador.
2. VERIFICA el resultado abriendo el PDF: ¿1-2 páginas exactas? ¿ninguna tarjeta ni renglón cortado entre páginas? ¿precios y sumas con formato $1,500,000? ¿cero siglas y cero tecnicismos? ¿nombre del cliente y del agente correctos?
3. Si algo falla, corrige y re-renderiza. Nunca entregues sin haber visto el PDF final.

## PASO 4 — Entregar y registrar

1. Entrega el PDF al agente (SendUserFile) con una línea: listo para mandar por WhatsApp.
2. Si el prospecto está en Contactos (búscalo SOLO entre las filas del agente): añade a Tema `fldraERshpAO7rRuj` (el campo de notas del kit) `[propuesta:FECHA aseguradoras]` — nunca borres notas previas. Si el Estatus era "Respondio", súbelo a "Cita" solo si ya hubo cita; no lo muevas solo por la propuesta.
3. Telemetría: en la fila de Actividad Diaria del día (créala con su nombre si falta), anota en Notas "propuesta: <negocio>".
4. Sugiérele el mensaje de envío (2 líneas, de usted, canon del kit): "Le mando la propuesta que le prometí — está en una página y sin letras chiquitas. Cualquier duda me dice y lo vemos en 10 minutos."
