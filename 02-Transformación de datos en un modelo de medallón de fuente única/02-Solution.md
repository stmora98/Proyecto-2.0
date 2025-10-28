# 🚀 **Reto 2: Guía de Conversión y Unificación**

## 🧩 **Convertir archivo JSON a CSV: Código**

```python
import json
import csv

# Ruta de entrada JSON
json_path = r"C:\Users\msalasrobles\Documents\compras_clientes.json"
# Ruta de salida CSV
csv_path = r"C:\Users\msalasrobles\Documents\compras_clientes_convertido.csv"

# Leer JSON
with open(json_path, encoding='utf-8') as f:
    data = json.load(f)

# Extraer encabezados desde el primer objeto
headers = list(data[0].keys())

# Escribir CSV
with open(csv_path, mode='w', newline='', encoding='utf-8') as f:
    writer = csv.DictWriter(f, fieldnames=headers)
    writer.writeheader()
    writer.writerows(data)

print(f"Archivo CSV generado en: {csv_path}")
```

## 🗃️ **Unificar Archivos CSV**

```python
import pandas as pd

# Rutas de los archivos
csv1 = r"path\clientes.csv"
csv2 = r"path\ventas.csv"
output_path = r"C:\Users\msalasrobles\Documents\dataset_unificado.csv"

# Leer ambos CSV como DataFrames
df_compras = pd.read_csv(csv1)
df_clientes = pd.read_csv(csv2)

# Unificar por customerId
df_unificado = pd.merge(df_compras, df_clientes, on='customerId', how='inner')

# Guardar resultado
df_unificado.to_csv(output_path, index=False, encoding='utf-8')

print(f"Dataset unificado guardado en: {output_path}")
```

## ✅ **Resultado esperado**
- Un archivo CSV llamado  con columnas combinadas de compras y datos demográficos.
- Listo para cargar en la capa Silver de Fabric para limpieza y enriquecimiento en el Reto 3.

---

# Solución Reto 02 — Transformación Intermedia y Análisis Exploratorio (Capa Silver)

Objetivo
- Transformar los datos de `bronze` a `silver`, ejecutar análisis exploratorio y dejar los datos listos para la capa Gold.

Requisitos previos
- Datos en `bronze` (Reto 01 completado).
- Acceso a Notebooks en Fabric (PySpark o Spark SQL).

## Pasos

### 1 — Crear tabla Silver a partir de Bronze

1. Abre un Notebook en Fabric (PySpark o Spark SQL).
2. Carga la tabla `bronze.sales` o el dataset correspondiente:

```python
# ejemplo (PySpark)
df = spark.read.table("bronze.sales")
display(df.limit(5))
```

3. Aplica limpieza adicional: tipos correctos, unificar nombres de columnas, eliminar duplicados.
4. Escribe el resultado como `silver.sales_clean` en la Lakehouse.

### 2 — Transformaciones intermedias de valor analítico

Aplica transformaciones que aporten valor:
- Agregaciones (totales por producto, por cliente).
- Creación de flags o segmentaciones (`high_value_customer`, `top_selling`).
- Conversión y jerarquías de categorías.

Ejemplo (PySpark):

```python
from pyspark.sql.functions import col, when

df2 = df.withColumn('sales_amount', col('quantity') * col('unit_price')) \
    .withColumn('is_high_value', when(col('sales_amount') > 1000, 1).otherwise(0))
```

### 3 — Análisis exploratorio y ML (K-Means como ejemplo)

1. Prepara features numéricas y normalízalas.
2. Usa PySpark MLlib o scikit-learn (en el notebook) para ejecutar K-Means.
3. Evalúa silueta, inercia y revisa clusters para interpretación.

Ejemplo rápido (scikit-learn):

```python
# extraer a pandas (si dataset pequeño)
pdf = df2.select('customer_id','sales_amount').toPandas()
from sklearn.cluster import KMeans
km = KMeans(n_clusters=3, random_state=42).fit(pdf[['sales_amount']])
pdf['cluster'] = km.labels_
```

4. Guarda resultados como `silver.customer_segments`.

### 4 — Validar y documentar

- Validar conteos y cambios con respecto a Bronze.
- Documentar transformaciones y parámetros de ML.

## Resultados esperados
- `silver.*` con tablas limpias y columnas de valor analítico.
- Clusters y perfiles listos para alimentar Gold.
