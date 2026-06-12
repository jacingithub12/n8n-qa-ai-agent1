# Sistema Modular de Ingesta de Datos y Agente de IA con n8n

Este repositorio contiene un sistema automatizado de dos etapas desarrollado en **n8n** para la gestión inteligente de preguntas y respuestas (Q&A). El proyecto demuestra habilidades en automatización lógica, estructuración de bases de datos internas (*Data Tables*) y orquestación de **Agentes de IA (Agentic AI)** con recuperación de información en tiempo real sin alucinaciones.

---

## 🚀 Arquitectura del Sistema

El proyecto está diseñado bajo un enfoque modular, separando la ingesta y enriquecimiento de datos de la lógica del bot de atención. Esto permite escalar la base de conocimientos de manera independiente sin afectar la disponibilidad del agente de IA.

### 1. Flujo de Ingesta (`QA Ingest`)
* **Trigger:** Recepción de datos a través de un formulario nativo de n8n.
* **Lógica Condicional (If Node):** Validación del dominio del correo electrónico del remitente para determinar de forma automática si es una fuente confiable (`isTrusted: true/false`).
* **Enriquecimiento con IA (Basic LLM Chain):** Un modelo de lenguaje analiza el contenido ingresado para generar etiquetas (*tags*) semánticas clave, optimizando la posterior búsqueda del bot.
* **Almacenamiento:** Inserción ordenada de los registros enriquecidos dentro de una base de datos interna (*n8n Data Tables*).
 <img width="1562" height="876" alt="HR Dashboard Home" src="QA Ingest Workflow.png" />

### 2. Flujo del Agente de IA (`QA AI Agent`)
* **Interface:** Trigger de chat interactivo nativo integrado con el *Chat Hub* de n8n.
* **Memoria Integrada (Simple Memory):** Mantiene el contexto de las conversaciones de forma síncrona basándose en el identificador de sesión (`sessionId`).
* **Uso de Herramientas (Tools):** El agente cuenta con una herramienta personalizada para consultar la base de datos interna mediante filtros avanzados de coincidencia de texto (*contains*) en las columnas de preguntas y etiquetas.
* **Garantía Anti-Alucinación:** El sistema de instrucciones fuerza al agente a notificar al usuario si no encuentra información respaldada en la base de datos, evitando respuestas inventadas.
<img width="1562" height="876" alt="HR Dashboard Home" src="QA AI Agent Workflow.png" />

---
## 💬 Resultado Final (Chat Hub)
Opcionalmente, se integró el flujo con el Chat Hub de n8n para permitir una interfaz de usuario interactiva y fluida:

 <img width="1562" height="876" alt="HR Dashboard Home" src="QA Chatbot.png" />

## 🛠️ Tecnologías Utilizadas

* **n8n Cloud / Self-hosted** (Versión 2.x)
* **OpenAI API** (Integración de modelos avanzados de lenguaje)
* **n8n Data Tables** (Base de datos relacional interna)
* **JSON** (Estructura para la portabilidad de los flujos)

---

## 📦 Cómo Replicar este Proyecto

Para importar y ejecutar este sistema en tu propia instancia de n8n, seguí estos pasos:

1. **Configurar la Base de Datos:**
   * Creá una nueva *Data Table* en n8n llamada `q&a`.
   * Añadí las siguientes columnas con sus respectivos tipos de datos:
     * `id` (Número/Autoincremental)
     * `Question` (Texto)
     * `Name` (Texto)
     * `Email` (Texto)
     * `Answer` (Texto)
     * `Tags` (Texto)
     * `isTrusted` (Booleano)

2. **Importar los Workflows:**
   * Cloná este repositorio o descargá los archivos `.json` dentro de la carpeta `workflows/`.
   * En tu panel de n8n, creá un flujo vacío, hacé clic en las opciones del lienzo y seleccioná **Import from File** (o simplemente arrastrá el archivo JSON sobre el lienzo).
   * Repetí el proceso para ambos flujos (`QA Ingest` y `QA AI Agent`).

3. **Credenciales:**
   * Vinculá tu propia API Key de OpenAI en los nodos de los modelos de chat.
   * ¡Activá (*Publish*) ambos flujos y empezá a automatizar!
