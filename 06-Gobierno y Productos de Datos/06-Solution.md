# Solución Reto 06 — Gobierno de Datos y Data Products (Purview + Fabric)

Objetivo
- Implementar catalogación y gobierno con Microsoft Purview, integrar Fabric como fuente y publicar Data Products documentados.

Requisitos previos
- Permisos para crear recursos en Purview y registrar fuentes.

## Pasos

### 1 — Provisionar Microsoft Purview

1. En el portal de Azure crea una cuenta de Purview (o usa la existente).
2. Accede al Data Map y crea una colección `ContosoData`.

### 2 — Registrar y escanear fuentes (Lakehouse en Fabric)

1. En Purview → Sources → Register new source → elegir el recurso de Fabric (Lakehouse o Warehouse).
2. Configura autenticación (Managed Identity o credenciales) y ejecuta un scan.
3. Revisa el Data Map para confirmar descubrimiento de tablas y linaje.

### 3 — Clasificar y crear glosario

1. Ejecuta clasificadores automáticos y añade clasificadores manuales para datos sensibles.
2. Crea un glosario de negocio con términos (`Cliente`, `Venta`, `Producto`) y documenta su significado.
3. Asocia términos del glosario a los activos catalogados.

### 4 — Crear Data Products en Fabric

1. En Fabric crea un producto de datos (ej. `Sales Insights Product`) combinando `customers` y `sales`.
2. Documenta en Purview: propósito, responsables, calidad, linaje y SLA.

### 5 — Validación y gobernanza continua

1. Prueba la búsqueda de activos desde Purview.
2. Verifica linaje desde origen (Lakehouse Bronze) hasta producto final (Gold/Data Product).
3. Genera un informe con: activos catalogados, términos creados, data products publicados.

## Resultados esperados
- Purview con activos y linaje visible.
- Data Products publicados y documentados con responsables y métricas de calidad.

---

Referencias
- Purview docs: https://learn.microsoft.com/purview
