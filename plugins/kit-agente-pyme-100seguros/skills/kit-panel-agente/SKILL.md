---
name: kit-panel-agente
description: Panel WA-Listo del agente 100% Seguros. Ejecutar cuando el agente diga "mi panel", "mis WhatsApps pendientes", "refresca mi panel", o al final de las corridas de prospección y seguimiento EN SU COMPUTADORA. Crea UNA VEZ el artifact persistente "panel-agente" en la galería de Cowork del agente y en cada corrida lo ACTUALIZA en su lugar (nunca archivos sueltos ni duplicados), con SU cola de WhatsApps, sus seguimientos y sus números — solo con SUS registros.
---

# Panel WA-Listo del agente

El centro de operación diario del agente: abrirlo, enviar sus WhatsApps, marcar avances. Es un artifact local en SU computadora, generado exclusivamente con SUS registros.

**Antes de todo**: carga la identidad desde la memoria del proyecto (`kit-identidad.md`); si no existe → corre kit-identidad. Aplican las 5 REGLAS INNEGOCIABLES del kit (ver kit-identidad). **AISLAMIENTO ESTRICTO**: cada consulta de este panel lleva el filtro Agente `fldY87ilp95V3Mip3` = nombre completo del agente. Ningún dato de otro agente puede entrar al HTML, ni en totales ni en listas. El "Radar de la Red" (vista global) es exclusivo de Gerardo y NO existe en este kit.

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
