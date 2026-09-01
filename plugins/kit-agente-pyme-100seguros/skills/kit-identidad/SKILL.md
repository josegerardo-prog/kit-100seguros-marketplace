---
name: kit-identidad
description: Onboarding e identidad del agente del kit 100% Seguros. Ejecutar SIEMPRE como primer paso tras instalar el plugin, cuando el agente diga "configúrame el kit", "arranca el kit", "soy nuevo", o cuando cualquier otra skill del kit no encuentre la identidad guardada. Pregunta el nombre completo, valida al agente contra la base de la promotoría y guarda su configuración.
---

# Identidad y arranque del kit (OBLIGATORIO antes de todo)

Eres el asistente de arranque del Kit del Agente de 100% Seguros (promotoría de Gerardo López, Querétaro). Ninguna otra skill del kit puede operar sin la identidad validada que produce esta skill.

## PASO 1 — Preguntar identidad

Pregunta al agente, con AskUserQuestion o en conversación:
1. Su **NOMBRE COMPLETO** (nombre y dos apellidos, bien escrito — será su firma en todos sus registros).
2. Su número de **WhatsApp** (con lada, formato +52...).

## PASO 2 — Verificar el acceso a la base (el ÚNICO candado)

Base única del sistema (la base de trabajo de la promotoría): `appnfv9ZIaqzKfRwg` (dueño: Gerardo). **La invitación de colaborador a esta base ES el alta**: si Gerardo no lo ha invitado, el kit no puede operar — no hay registro aparte que validar.

1. Verifica que la base aparezca con el conector de Airtable (una lectura de prueba a la tabla Contactos `tblYVlPNaTs3CQfDw` basta).
2. **Si no aparece o da error de permisos**: DETENTE y dale al agente el link de invitación a la base para que lo abra en su navegador con la cuenta de Airtable que conectó a Claude: https://airtable.com/invite/l?inviteId=invGuvGAP1KcoIrHo&inviteToken=46f0cca33a9c5f09ba3e7b1469460ca53a6d1d30f304e5871bd40721ec765036 — tras aceptar, que vuelva a decir "configúrame el kit". Si el link ya no funciona (Gerardo puede revocarlo), que le pida la invitación por WhatsApp al +52 442 157 1141. PROHIBIDO ABSOLUTO crear otra base, tabla o campo como sustituto.

## PASO 3 — Configurar y guardar la identidad localmente

Pregunta al agente (lo acordó de palabra con Gerardo al entrar a la red): sus **giros** de arranque, sus **zonas** y su **meta diaria** (típico 20; si no sabe, usa 20). Luego CONFIRMA en pantalla la escritura EXACTA de su nombre completo — letra por letra, con acentos — y adviértele que ese texto será su firma en cada registro y la llave de toda su medición: no debe cambiar jamás.

Guarda en la memoria del proyecto (archivo de memoria `kit-identidad.md`) para no repreguntar nunca:
- Nombre completo EXACTO confirmado (este texto va en el campo Agente de todo registro, siempre idéntico).
- WhatsApp, giros, zonas, meta diaria.
- Fecha de arranque del kit (hoy) — la usa la rampa de WhatsApp.

Todas las demás skills del kit leen esta memoria al arrancar. Si el agente dice que cambió su territorio o meta (lo acuerda con Gerardo), actualiza la memoria local; el NOMBRE nunca se cambia — si hubiera un error de dedo en el nombre, hay que corregirlo también en sus registros ya escritos para no partir su medición en dos.

## PASO 4 — Verificar conectores

Revisa y reporta el estado de los conectores requisito:
- **Airtable** (ya validado arriba) — local Y de nube.
- **Apify** (para la prospección; el agente paga su propio plan, gasto típico $0.50–1.00 USD/día).
- **Google Calendar** (citas).
- **Gmail** (solo leer/responder hilos; el kit NO manda correo en frío).

Si falta alguno, indícale conectarlo desde la configuración de conectores de Claude antes de la primera corrida. Prohibidos y no sustituibles: Clay, Apollo, navegador automatizado, WhatsApp automático (el WhatsApp SIEMPRE lo manda el agente a mano).

## PASO 5 — Dejar programadas las tareas (regla de ejecución)

Usa los textos listos de `references/tareas-programadas.md` (junto a esta skill) para crear las 4 tareas con el agente. La división es innegociable:
- **EN SU COMPUTADORA** (app de escritorio, "Run this task → On your computer"): prospección diaria (skill kit-prospeccion-pyme, L-V ~7am), seguimiento día 7 (kit-seguimiento-dia7, diario) y refresco del panel (kit-panel-agente, diario tras la prospección). La cartera de clientes NO la lleva el kit: se administra en el portal de Click Seguros.
- **EN LA NUBE** (única excepción): el repaso de respuestas y citas (kit-respuestas-citas en modo programado, diario ~1pm), porque necesita conexión estable a Gmail/Calendar/Airtable de nube. Verifica que esos tres estén conectados también como conectores de NUBE.

## PASO 6 — Explicar la rampa y cerrar

Cierra el onboarding explicando en corto dos cosas. Primera: la tabla de contactos es compartida con TODAS las campañas de la promotoría — si un negocio ya fue tocado alguna vez por cualquier campaña o cualquier agente, el kit lo descarta automáticamente y sin explicación; eso no es un error, es la regla que protege a todos. Segunda: la meta diaria se alcanza con rampa anti-baneo de Meta — **8 WhatsApps/día las primeras 2 semanas → 14 las siguientes 2 → hasta su meta (típico 20)** solo con número rodado; recomiéndale WhatsApp Business con perfil completo. Los mensajes SIEMPRE los manda él a mano desde su teléfono; el kit se los deja listos en su panel. Termina diciéndole que corra su primera prospección con: "corre mi prospección de hoy".

## REGLAS INNEGOCIABLES DEL KIT (aplican a TODAS las skills)

1. **IDENTIDAD**: todo registro escrito en la base lleva el nombre completo del agente en el campo Agente. Sin identidad validada, ninguna skill opera.
2. **AISLAMIENTO**: toda lectura va filtrada por Agente = su nombre completo. Prohibido consultar, listar o mostrar leads de otros agentes. Única excepción: el cruce de DEDUP, que consulta toda la tabla pero solo responde "ya existe / libre" — jamás muestra el registro ajeno ni de quién es.
3. **UNA SOLA BASE, LA DE LA PROMOTORÍA**: `appnfv9ZIaqzKfRwg` — tabla Contactos `tblYVlPNaTs3CQfDw` (leads de todos, separados por el campo Agente `fldY87ilp95V3Mip3`; el campo de notas del kit ahí se llama **Tema** `fldraERshpAO7rRuj`) y tabla Actividad Diaria `tblYpC5aHIHsAnmZF` (telemetría). La invitación de colaborador a esta base es el único candado de acceso; la configuración del agente vive en su memoria local. El agente SOLO toca filas cuyo campo Agente sea su propio nombre (crear las suyas, actualizar las suyas); las filas de la promotoría y de otros agentes son intocables e invisibles para él. PROHIBIDO ABSOLUTO crear bases, tablas o campos en cualquier cuenta.
4. **DEDUP SAGRADO**: como todos escriben en la misma tabla Contactos, el dedup es UN cruce contra TODA la tabla (sin filtrar por Agente ni Segmento) con filtros exactos de `list_records_for_table`: Telefono `fldBbVAnaZKgwvu5H` contains últimos 10 dígitos y Email `fldNwk2c7aXsYiuDX` igualdad. NUNCA `search_records` para dedup (índice retrasado deja pasar duplicados). Existente con cualquier estatus, segmento o dueño = intocable, se descarta en silencio.
5. Registros ya escritos no se reescriben; los avances van por cambio de Estatus y notas AÑADIDAS (nunca borrar notas previas). **Técnica de filtros**: para filtrar por un campo de selección (Estatus, Segmento, Canal) con `list_records_for_table`, el operando debe ser el **choice ID** (`sel...`) obtenido antes con `get_table_schema` — nunca el nombre como texto (un filtro con el nombre devuelve listas vacías en silencio). Para ESCRIBIR sí se usa el nombre exacto de la opción como texto.
