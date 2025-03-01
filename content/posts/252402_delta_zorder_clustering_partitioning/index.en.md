---
title: "Delta Lake — Z-Ordering, Partitioning, Liquid Clustering and Bucketing (Part 1)"
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
This is a re-translated and edited article.
Find the original article [here](https://medium.com/towards-data-science/delta-lake-partitioning-z-order-and-liquid-clustering-944030ff1828).
{{< /alert >}} -->

</br>

## Introduction

One of the key reasons why big data remains an attractive and crucial field lies in its very name - **BIG**. The **big** aspect is also what makes working in this domain challenging and complex. As a result, people are constantly searching for ways to optimize their workflows when handling big data.


For data engineers and professionals working with big data, Databricks and Delta Lake are likely familiar names. As mentioned earlier, optimizing performance is essential when dealing with big data. That’s why I want to introduce some optimization techniques I have learned through my studies and work with Databricks and Delta Lake. You should have some basic knowledge of Apache Spark as well as Distributed Systems to understand this article.


Let’s dive into four key optimization techniques: **Partitioning**, **Z-Ordering**, **Bucketing** and **Liquid Clustering**. Each plays a unique role in organizing and accessing data more efficiently.

### 1. Partitioning: Divide and Conquer

Partitioning is one of the most fundamental optimization techniques in big data. Instead of storing data in a single large table, we split it into smaller partitions based on a chosen column. This dramatically improves query performance, as Spark can skip unnecessary partitions when reading data.

#### Example: Partitioning by Year and Month
Imagine you have a sales dataset with billions of rows. Queries often filter by year and month. Instead of scanning the entire dataset, you can partition by year and month, so Spark only reads relevant files.

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

{{< icon "lightbulb" >}} When to use Partitioning:
- **Frequent Filtering**: Queries often filter (`WHERE`) by a specific column (low cardinality).
- **Scalability**: You have a large dataset that needs to be split

{{< icon "triangle-exclamation" >}} When not to use Partitioning:
- **Small Datasets**: Partitioning may not be necessary for small datasets.


### 2. Z-Ordering: The Secret of Data Skipping
Partitioning is quite effective, but what if your query often filters by **multiple columns**? This is where Z-Ordering comes in. Z-Ordering is a technique that reorganizes data in a way that makes it easier to skip unnecessary partitions when querying multiple columns.
It clusters data within partitions based on multiple columns, making range-based queries more efficient. In Databricks, you can use the `OPTIMIZE` command to apply Z-Ordering to your Delta Lake table.

#### Example: Using Z-Ordering
```sql
OPTIMIZE table_name 
ZORDER BY (column_1, column_2, column_3);
```


{{< icon "lightbulb" >}} When to use Z-Ordering:
- **Multiple Column Filtering**: Queries often filter by multiple columns.
- **Range Queries**: Queries often involve range-based filters.


{{< icon "triangle-exclamation" >}} When not to use Partitioning:
- **Single Column Filtering**: If queries only filter by a single column, Z-Ordering may not be necessary.


### 3. Bucketing: Pre-Sorting for Faster Joins
If your queries often involve joins especially on large tables, especially on large tables, Bucketing can help speed up the process. Instead of just partitioning, bucketing pre-sort data into fixed-size buckets based on a hash function. This make joins more efficient as Spark can skip unnecessary data when joining tables, reducing the amount of data shuffled across the network.

Bucketing have some similarities with Partitioning, but it has a different purpose. While Partitioning is used to divide data into smaller partitions, Bucketing is used to pre-sort data into fixed-size buckets. This makes it more suitable for joins, groupings, and aggregations While Partitioning is more suitable for filtering or scanning data.

#### Example: Bucketing by Customer ID into 10 Buckets
```sql
CREATE TABLE sales_data (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE
)
USING DELTA
CLUSTERED BY (customer_id) INTO 10 BUCKETS;
```

{{< icon "lightbulb" >}} When to use Bucketing:
- **Join-heavy Queries**: Queries often involve joins.
- **Grouping and Aggregations**: Queries often involve grouping and aggregations.

{{< icon "triangle-exclamation" >}} When not to use Bucketing:
- **Single Table Queries**: If your queries only involve a single table, Bucketing may not be necessary.


### 4. Liquid Clustering: The Best of Both Worlds
Liquid Clustering is a new and dynamic alternative to traditional Partitioning and Bucketing. Unlike static partitions, it automatically reorganizes data based on the query patterns. This makes it more flexible and adaptive to different query patterns.

Liquid Clustering is a game changer for workloads with varying query patterns. It combines the benefits of Partitioning and Bucketing, making it suitable for a wide range of queries. You don't need to pre-define partitions or buckets, as Liquid Clustering automatically adapts to your query patterns.
It make use of the `OPTIMIZE` command to apply Liquid Clustering to your Delta Lake table.

#### Example: Using Liquid Clustering
```sql
CREATE TABLE sales_data (
    order_id STRING,
    customer_id STRING,
    amount DOUBLE
)
USING DELTA
CLUSTERED BY (customer_id)
```
{{< icon "lightbulb" >}} When to use Liquid Clustering:
- **Varying Query Patterns**: Workloads with varying query patterns.
- **Adaptive Workloads**: Workloads that require dynamic optimization.

{{< icon "triangle-exclamation" >}} When not to use Liquid Clustering:
- **Static Workloads**: If your query patterns are consistent, Liquid Clustering may not be necessary.
- **Small Datasets**: Liquid Clustering may not be necessary for small datasets.



## Conclusion
Optimizing big data workloads is essential for performance and efficiency. By leveraging techniques like Partitioning, Z-Ordering, Bucketing, and Liquid Clustering, you can improve query performance and reduce data processing time. Each technique has its unique use cases and benefits, so choose the one that best fits your workload.