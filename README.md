# Marketplace 100% Seguros

Repositorio de distribución de las herramientas internas de la red de agentes de **100% Seguros — Querétaro**.

Contacto: Gerardo López · WhatsApp +52 442 157 1141 · josegerardo@100seguros.com.mx

---

## Para el agente: cómo instalar

En Claude (Cowork de escritorio), una sola vez:

```
/plugin marketplace add josegerardo-prog/kit-100seguros-marketplace
/plugin install kit-agente-pyme-100seguros@100seguros
```

Después:

```
configúrame el kit
```

Eso corre `kit-identidad`: te pregunta tu nombre completo, tu territorio y tu meta, verifica tus conectores y te deja programadas las tareas diarias.

### Antes de instalar necesitas

1. Plan de Claude con Cowork de escritorio (lo pagas tú).
2. Conectores: **Airtable, Apify, Google Calendar y Gmail**. Airtable, Gmail y Calendar conéctalos también como conectores de nube. Apify lo pagas tú (~$0.50–1.00 USD por corrida).
3. **Invitación a la base de Airtable de la promotoría** — pídesela a Gerardo. Sin aceptarla el kit no arranca.
4. Tu territorio acordado con Gerardo (giros, zonas, meta diaria).
5. WhatsApp Business con perfil completo.

### Cómo actualizar

Cuando Gerardo avise que hay versión nueva:

```
/plugin marketplace update 100seguros
```

---

## Para Gerardo: cómo publicar cambios

1. Edita lo que toque dentro de `plugins/kit-agente-pyme-100seguros/`.
2. Sube la versión en **los dos** archivos:
   - `plugins/kit-agente-pyme-100seguros/.claude-plugin/plugin.json` → campo `version`
   - `.claude-plugin/marketplace.json` → `version` de la entrada del plugin
3. `git add -A && git commit -m "v1.2.0: qué cambió" && git push`
4. Avisa a la red que corran `/plugin marketplace update 100seguros`.

> **Ojo:** si no subes el `version` dentro de `marketplace.json`, los agentes se quedan con la copia vieja en caché aunque hayas hecho push. Ese es el error clásico.

## Estructura

```
.claude-plugin/
  marketplace.json                      ← catálogo (fuente de verdad para distribuir)
plugins/
  kit-agente-pyme-100seguros/
    .claude-plugin/plugin.json          ← manifiesto del plugin
    README.md
    skills/
      kit-identidad/
      kit-prospeccion-pyme/
      kit-respuestas-citas/
      kit-seguimiento-dia7/
      kit-propuesta-pdf/
      kit-panel-agente/
```
