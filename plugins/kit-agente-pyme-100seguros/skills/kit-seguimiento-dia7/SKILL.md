---
name: kit-seguimiento-dia7
description: Seguimiento día 7 del agente 100% Seguros. Ejecutar cuando el agente diga "dame mis seguimientos", "quién no me ha contestado", o desde su tarea programada diaria EN SU COMPUTADORA (después de la prospección). Encuentra sus contactados sin respuesta a los 7 días y deja el segundo WhatsApp redactado — el toque que más respuesta genera.
---

# Seguimiento día 7 (el segundo toque)

El seguimiento a quien no contestó ~duplica la tasa de respuesta. Un solo seguimiento por lead, ni más ni menos: dignidad ante todo.

**Antes de todo**: carga la identidad desde la memoria del proyecto (`kit-identidad.md`); si no existe → corre kit-identidad. Aplican las 5 REGLAS INNEGOCIABLES del kit (ver kit-identidad).

## PASO 1 — Encontrar candidatos (solo SUYOS)

`list_records_for_table` en Contactos `tblYVlPNaTs3CQfDw` (base `appnfv9ZIaqzKfRwg`) con filtro: **Agente `fldY87ilp95V3Mip3` = nombre completo del agente** Y Estatus `fldLJp1Ns7qEScBsN` = "Contactado" Y Fecha `fldUprZ41CLEPNLZz` entre hace 7 y hace 9 días. Descarta los que ya tengan `[seg7:` en Tema `fldraERshpAO7rRuj` (el campo de notas del kit) (ya se les dio seguimiento — jamás un segundo seguimiento) y los que tengan `[tibio:` con menos de 7 días (respetar su tiempo).

## PASO 2 — Redactar el segundo WhatsApp

Canon del seguimiento (30-45 palabras, de usted, sin presión, sin repetir el primer mensaje, sin "seguros"):
- Referencia ligera al primer mensaje + un elemento NUEVO: un dato del mapa de dolor de su giro (`${CLAUDE_PLUGIN_ROOT}/skills/kit-prospeccion-pyme/references/mapa-dolor-giros.md`) convertido en pregunta útil, o una novedad del negocio si la hay.
- Ejemplo de forma (varía siempre): "Hola, le escribí hace unos días sobre <negocio>. Le comparto algo que veo seguido en <giro>s: <dato útil en una línea>. Si le hace sentido platicarlo, aquí ando. Y si no es momento, ningún problema."
- Un lead con `[tibio:]` recibe versión aún más suave: solo el dato útil y la puerta abierta.

## PASO 3 — Registrar y entregar

1. En cada lead: añade a Tema `fldraERshpAO7rRuj` (el campo de notas del kit) `[seg7:FECHA]` + el mensaje redactado (nunca borres notas previas; el Estatus se queda en "Contactado").
2. Telemetría: en la fila de Actividad Diaria del día (créala con su nombre si no existe), anota en Notas "seguimientos día 7: N".
3. Entrega la lista: negocio, teléfono con link wa.me, y el mensaje listo para copiar. El agente los manda A MANO. Estos seguimientos NO cuentan contra el cupo de la rampa (son leads ya reservados), pero recomiéndale no pasar de ~30 WhatsApps totales al día entre nuevos y seguimientos.
4. Refresca el panel (skill kit-panel-agente) para que los seguimientos aparezcan en su sección.
