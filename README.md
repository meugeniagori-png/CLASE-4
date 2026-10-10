# Checkpoint 4: Sincronización del Cerebro Agéntico con Ecosistemas de Negocio

Este repositorio contiene el flujo de automatización desarrollado en **n8n** para el Checkpoint 4 del curso de Inteligencia Artificial. El workflow integra un sistema de triaje agéntico multi-canal conectando Gmail, HubSpot CRM, Slack y Google Sheets mediante protocolos de autenticación OAuth2.

---

## 🛠️ Arquitectura del Workflow

El flujo procesa los correos de soporte entrantes, gestiona el contexto/memoria en Google Sheets, actualiza el CRM y prepara respuestas para revisión humana con las siguientes salvaguardas operativas:

1. **① Control Anti Auto-Reply (Bucle Infinito):**
   - Nodo IF que escanea el correo entrante y descarta automáticamente emails con asuntos como `Auto-reply`, `Out of office`, `Undeliverable` o provenientes de direcciones `no-reply@`.
   
2. **② Prevención de Duplicados en CRM (Error 409):**
   - Compuerta *Look up* en HubSpot que verifica si el contacto ya existe por su dirección de email.
   - Si existe, ejecuta un `Update`; si no existe, ejecuta un `Create`.

3. **③ Barrera Human-In-The-Loop (HITL):**
   - Mapeo hacia Gmail utilizando la operación **Create Draft** (Crear Borrador). La IA redacta la propuesta de respuesta pero **nunca envía el correo directamente** sin la aprobación final de un operador humano.

4. **④ Sanitización y Mínimo Privilegio (Evita Error 400 y Saturación):**
   - Nodos `Set` dedicados a limpiar y filtrar payloads antes de enviar datos a Slack y al LLM, garantizando mensajes ligeros y bien estructurados.

---

## 📄 Estructura del Proyecto

* `checkpoint4_nombre_apellido.json`: Workflow completo de n8n listo para importar (Workflow > Import from File).

---

## 🚀 Cómo importar en n8n

1. Descargá el archivo `.json` de este repositorio.
2. En tu instancia de n8n, andá a **Workflows** > **Import from File**.
3. Seleccioná el archivo y reconfigurá las credenciales necesarias (Gmail, HubSpot, Slack y Google Sheets).
