# Tareas programadas del kit — textos listos para crear (usar en el PASO 5 del onboarding)

Ayuda al agente a crear estas 4 tareas programadas EXACTAMENTE así. Las tres primeras se crean con "Run this task → **On your computer**" (app de escritorio); la última con "**In the cloud**". Ajusta solo los horarios si el agente lo pide.

## 1. Prospección diaria (EN SU COMPUTADORA, L-V 7:00 am)
Nombre: `Kit — Prospección PyME diaria`
Prompt:
> Corre mi prospección PyME de hoy con la skill kit-prospeccion-pyme del kit del agente. Ejecuta de corrido: cupo según mi rampa, giro y zona de mi territorio, descubre con Apify, deduplica contra la red, registra mi lote WA-Listo con mis mensajes redactados, deja la telemetría en Actividad Diaria y refresca mi panel.

## 2. Seguimiento día 7 (EN SU COMPUTADORA, L-V 7:30 am)
Nombre: `Kit — Seguimiento día 7`
Prompt:
> Corre mi seguimiento día 7 con la skill kit-seguimiento-dia7 del kit del agente: encuentra mis contactados sin respuesta de hace 7-9 días, redacta el segundo WhatsApp de cada uno, márcalos con [seg7:] y déjalos en mi panel.

## 3. Refresco del panel (EN SU COMPUTADORA, L-V 7:45 am — opcional si las dos anteriores ya lo refrescan)
Nombre: `Kit — Panel del día`
Prompt:
> Refresca mi panel con la skill kit-panel-agente del kit del agente: mi cola WA-Listo, seguimientos de hoy, quiénes respondieron y mis números. Solo mis registros.

## 4. Repaso de respuestas y citas (EN LA NUBE, diario 1:00 pm) — ÚNICA de nube
Nombre: `Kit — Repaso respuestas y citas`
Prompt:
> Corre el modo programado (modo B) de la skill kit-respuestas-citas del kit del agente: revisa MIS WA-Listos rezagados de más de 48 horas, mis "Respondió" sin siguiente paso, mis citas de hoy y mañana en Google Calendar con mini-brief de cada una, y si algún prospecto mío me escribió por correo prepárame el borrador. Entrégame un resumen accionable de máximo 15 líneas.

Requisito de la tarea de nube: Airtable, Gmail y Google Calendar conectados como conectores de NUBE. Verifícalo al crearla.

Recuérdale al agente: las tareas en su computadora corren solo si la app de escritorio de Claude está abierta a esa hora — recomendación: dejarla abierta al empezar el día o correrlas a mano con "corre mi prospección".
