---
title: "Delta Lake — Z-Ordering, Particionamiento, Liquid Clustering y Bucketing (Parte 1)"
summary: ""
description: ""
categories: ["Delta", "Spark"]
tags: ["tutorial"]
date: 2025-02-24
draft: false
showauthor: false
authors:
  - ntphiep
---


<!-- {{< alert icon="circle-info" cardColor="#EB5B00" iconColor="#1d3557" textColor="#f1faee" >}}
Este es un artículo retraducido y editado.
Encuentra el artículo original [aquí](https://medium.com/towards-data-science/delta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828).
{{< /alert >}} -->

</br>

## Introducción

Una de las razones clave por las que el big data sigue siendo un campo atractivo y crucial radica en su propio nombre - **BIG** (GRANDE). El aspecto **grande** es también lo que hace que trabajar en este dominio sea desafiante y complejo. Como resultado, las personas buscan constantemente formas de optimizar sus flujos de trabajo al manejar big data.


Para ingenieros de datos y profesionales que trabajan con big data, Databricks y Delta Lake son probablemente nombres familiares. Como se mencionó anteriormente, optimizar el rendimiento es esencial cuando se trabaja con big data. Por eso quiero presentar algunas técnicas de optimización que he aprendido a través de mis estudios y trabajo con Databricks y Delta Lake. Deberías tener conocimientos básicos de Apache Spark así como de Sistemas Distribuidos para entender este artículo.


Vamos a profundizar en cuatro técnicas clave de optimización: **Particionamiento**, **Z-Ordering**, **Bucketing** y **Liquid Clustering**. Cada una juega un papel único en la organización y acceso a los datos de manera más eficiente.

### 1. Particionamiento: Divide y Vencerás

El particionamiento es una de las técnicas de optimización más fundamentales en big data. En lugar de almacenar datos en una sola tabla grande, los dividimos en particiones más pequeñas basadas en una columna elegida. Esto mejora dramáticamente el rendimiento de las consultas, ya que Spark puede omitir particiones innecesarias al leer datos.

#### Ejemplo: Particionamiento por Año y Mes
Imagina que tienes un conjunto de datos de ventas con miles de millones de filas. Las consultas a menudo filtran por año y mes. En lugar de escanear todo el conjunto de datos, puedes particionar por año y mes, para que Spark solo lea los archivos relevantes.

```sql
CREATE TABLE sales_data (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE,
    year INT,
    month INT
) 
USING DELTA 
PARTITIONED BY (year, month);
```

{{< icon "lightbulb" >}} Cuándo usar el Particionamiento:
- **Filtrado Frecuente**: Las consultas a menudo filtran (`WHERE`) por una columna específica (baja cardinalidad).
- **Escalabilidad**: Tienes un conjunto de datos grande que necesita ser dividido

{{< icon "triangle-exclamation" >}} Cuándo no usar el Particionamiento:
- **Conjuntos de Datos Pequeños**: El particionamiento puede no ser necesario para conjuntos de datos pequeños.


### 2. Z-Ordering: El Secreto del Salto de Datos
El particionamiento es bastante efectivo, pero ¿qué pasa si tu consulta a menudo filtra por **múltiples columnas**? Aquí es donde entra Z-Ordering. Z-Ordering es una técnica que reorganiza los datos de una manera que facilita el salto de particiones innecesarias al consultar múltiples columnas.
Agrupa datos dentro de particiones basándose en múltiples columnas, haciendo que las consultas basadas en rangos sean más eficientes. En Databricks, puedes usar el comando `OPTIMIZE` para aplicar Z-Ordering a tu tabla Delta Lake.

#### Ejemplo: Usando Z-Ordering
```sql
OPTIMIZE table_name 
ZORDER BY (column_1, column_2, column_3);
```


{{< icon "lightbulb" >}} Cuándo usar Z-Ordering:
- **Filtrado por Múltiples Columnas**: Las consultas a menudo filtran por múltiples columnas.
- **Consultas de Rango**: Las consultas a menudo involucran filtros basados en rangos.


{{< icon "triangle-exclamation" >}} Cuándo no usar el Particionamiento:
- **Filtrado por una Sola Columna**: Si las consultas solo filtran por una sola columna, Z-Ordering puede no ser necesario.


### 3. Bucketing: Pre-Ordenamiento para Joins Más Rápidos
Si tus consultas a menudo involucran joins especialmente en tablas grandes, especialmente en tablas grandes, Bucketing puede ayudar a acelerar el proceso. En lugar de solo particionar, el bucketing pre-ordena los datos en buckets de tamaño fijo basados en una función hash. Esto hace que los joins sean más eficientes ya que Spark puede omitir datos innecesarios al unir tablas, reduciendo la cantidad de datos mezclados a través de la red.

Bucketing tiene algunas similitudes con el Particionamiento, pero tiene un propósito diferente. Mientras que el Particionamiento se usa para dividir datos en particiones más pequeñas, Bucketing se usa para pre-ordenar datos en buckets de tamaño fijo. Esto lo hace más adecuado para joins, agrupaciones y agregaciones. Mientras que el Particionamiento es más adecuado para filtrar o escanear datos.

#### Ejemplo: Bucketing por ID de Cliente en 10 Buckets
```sql
CREATE TABLE sales_data (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE
)
USING DELTA
CLUSTERED BY (customer_id) INTO 10 BUCKETS;
```

{{< icon "lightbulb" >}} Cuándo usar Bucketing:
- **Consultas con Muchos Joins**: Las consultas a menudo involucran joins.
- **Agrupación y Agregaciones**: Las consultas a menudo involucran agrupación y agregaciones.

{{< icon "triangle-exclamation" >}} Cuándo no usar Bucketing:
- **Consultas de una Sola Tabla**: Si tus consultas solo involucran una sola tabla, Bucketing puede no ser necesario.


### 4. Liquid Clustering: Lo Mejor de Ambos Mundos
Liquid Clustering es una alternativa nueva y dinámica al Particionamiento y Bucketing tradicionales. A diferencia de las particiones estáticas, reorganiza automáticamente los datos basándose en los patrones de consulta. Esto lo hace más flexible y adaptable a diferentes patrones de consulta.

Liquid Clustering es un cambio revolucionario para cargas de trabajo con patrones de consulta variables. Combina los beneficios del Particionamiento y Bucketing, haciéndolo adecuado para una amplia gama de consultas. No necesitas predefinir particiones o buckets, ya que Liquid Clustering se adapta automáticamente a tus patrones de consulta.
Hace uso del comando `OPTIMIZE` para aplicar Liquid Clustering a tu tabla Delta Lake.

#### Ejemplo: Usando Liquid Clustering
```sql
CREATE TABLE sales_data (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE
)
USING DELTA
CLUSTERED BY (customer_id)
```
{{< icon "lightbulb" >}} Cuándo usar Liquid Clustering:
- **Patrones de Consulta Variables**: Cargas de trabajo con patrones de consulta variables.
- **Cargas de Trabajo Adaptativas**: Cargas de trabajo que requieren optimización dinámica.

{{< icon "triangle-exclamation" >}} Cuándo no usar Liquid Clustering:
- **Cargas de Trabajo Estáticas**: Si tus patrones de consulta son consistentes, Liquid Clustering puede no ser necesario.
- **Conjuntos de Datos Pequeños**: Liquid Clustering puede no ser necesario para conjuntos de datos pequeños.



## Conclusión
Optimizar las cargas de trabajo de big data es esencial para el rendimiento y la eficiencia. Al aprovechar técnicas como Particionamiento, Z-Ordering, Bucketing y Liquid Clustering, puedes mejorar el rendimiento de las consultas y reducir el tiempo de procesamiento de datos. Cada técnica tiene sus casos de uso y beneficios únicos, así que elige la que mejor se adapte a tu carga de trabajo.
