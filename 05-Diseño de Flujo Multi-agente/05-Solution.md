# 🤖 **Guía de Preparación del Entorno para Azure AI Foundry**

---

## ⚙️ **Parte 1: Preparación del entorno**

### ✅ **Requisitos previos**

- Tener acceso a **Azure AI Studio**  
- Haber creado un **proyecto en Azure AI Foundry**  
- Contar con el **dataset enriquecido** disponible en formato CSV o como tabla en **Fabric**

---

### 🔹 **Paso 1: Crear un proyecto en Azure AI Foundry**

1. Accede a **Azure AI Studio → Foundry → Crear nuevo proyecto**  
2. Asigna un nombre (ejemplo: `Proyecto_ClientesAI`)  
3. Selecciona el tipo de proyecto: **Prompt Flow**  
4. Define el objetivo:  
   > “Generar insights narrativos y clasificaciones de clientes”

---

### 🔹 **Paso 2: Subir el dataset enriquecido**

1. En el proyecto, navega a **Data Assets**.  
2. Carga el archivo desde **Fabric** o desde tu equipo local.  
3. Verifica que los campos clave estén presentes.  

**Campos requeridos:**
- `customerId`
- `total_gastado`
- `frecuencia_compra`
- `pais`
- `fecha`

📂 **Ejemplo:**  
Carga el archivo **dataset_enriquecido.csv** desde Fabric o localmente.

---

## 🧠 **Parte 2: Diseño de los flujos de prompts**

🔹 **Reto 1: Generación de resúmenes de compra**

**Prompt base:**

Dado el siguiente cliente:  
- ID: {customerId}  
- Total gastado: {total_gastado}  
- Frecuencia de compra: {frecuencia_compra}  
- País principal: {pais}  

Genera un resumen breve del comportamiento de compra.  

**Salida esperada:**

> “El cliente C001 ha gastado $1,250 en 8 compras, siendo México su país principal de actividad.”

---

🔹 **Reto 2: Clasificación de clientes**

**Prompt base:**  

Clasifica al siguiente cliente según su comportamiento:  
- Total gastado: {total_gastado}  
- Frecuencia de compra: {frecuencia_compra}  

**Categorías posibles:**  
- Alto valor  
- Frecuente  
- Ocasional  

Justifica tu clasificación.  

**Salida esperada:**

> “Cliente C002 es clasificado como ‘Frecuente’ por realizar 12 compras con un gasto moderado de $980.”

---

🔹 **Reto 3: Generación de insights narrativos**

**Prompt base:**  

Genera un reporte narrativo mensual por país. Datos:  
- País: {pais}  
- Mes: {mes}  
- Total de compras: {total_compras}  
- Monto total: {monto_total}  

**Ejemplo de salida:**  
‘En septiembre, los clientes de México realizaron X compras por un total de Y.’  

**Salida esperada:**

> “En octubre, los clientes de Colombia realizaron 320 compras por un total de $45,000.”

---

## ⚙️ **Parte 3: Implementación en Prompt Flow**

**Paso a paso**

1. Crea un nuevo Flow por reto (puedes duplicar y adaptar).  
2. Usa el componente Data Input para conectar el CSV.  
3. Usa el componente LLM Prompt para definir el prompt.  
4. Usa el componente Output Parser para estructurar la respuesta.  
5. Ejecuta el Flow y valida los resultados.  

---

## ✅ **Parte 4: Resultado esperado**

- Tres flujos funcionales en Azure AI Foundry que aplican LLMs sobre datos enriquecidos.  
- Prompts reutilizables y adaptables para otros casos de negocio.  
- Base sólida para extender hacia recomendaciones, predicciones o generación de contenido personalizado.

---







# Solución Reto 05 — Diseño y Orquestación de Flujo Multi-agente

Objetivo
- Diseñar un flujo multi-agente (ingesta, análisis y decisión) con contratos de mensajes, manejo de errores y métricas de validación.

Requisitos previos
- Definición de fuentes de datos y esquema de mensajes.

## Pasos

### 1 — Definir roles y contratos

1. Documenta 3 agentes con entradas/salidas:
   - Ingesta: recibe raw, publica `NEW`.
   - Análisis: consume `NEW` → `SCORED`.
   - Decisión/Ejecución: consume `SCORED` → `EXECUTED`.
2. Define el contrato de mensajes (headers: `correlationId`, `timestamp`; payload: entidad/atributos; status).

### 2 — Diseñar el flujo y condiciones

1. Bosqueja el flujo: Ingesta → Análisis → Decisión.
2. Añade ramificaciones condicionales (p. ej., enrich si faltan datos, DLQ en errores irreparables).

### 3 — Implementar resiliencia

1. Estrategias:
   - Retries exponenciales para fallos transitorios.
   - Dead-letter queue para mensajes no procesables.
   - Timeouts y circuit-breakers.

### 4 — Simular y validar

1. Genera datos sintéticos que reproduzcan volúmenes y variabilidad.
2. Ejecuta simulaciones midiendo tiempo E2E (P50/P90), throughput y tasa de errores.

### 5 — Métricas y mejora continua

1. Exporta métricas a un tablero: latencia, errores, reintentos, éxito.
2. Define umbrales y runbooks para la operación.

## Documentación final
- Diagrama de flujo con IDs y condiciones.
- Contratos de mensajes versionados.
- Resultados de simulación y tablero de métricas.






