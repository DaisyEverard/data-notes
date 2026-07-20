
A data optimization technique for Delta lake. Replaces traditional partitioning and z-ordering instead using clustering keys to efficiently skip data.
Optimizes files for performance during pipeline runs. Uses a hilbert curve to make file boundaries tighter. I've added a picture of a hilbert curve because maths is cool.
![[hilbert.gif]]
Hive partitioning used a rigid directory structure requiring a full rewrite to change the keys. Z-ordering required manual OPTIMIZE runs with a full rewrite on every run. 

Liquid clustering is:
- Incremental - only optimizes new or unclustered data so works for streaming and write-heavy workloads
- Flexible - clustering keys can be updated at any time without full table rewrites. They can adapt to evolving query patterns
- Self-Tuning - with CLUSTER BY AUTO databricks will select optimal keys based on query usage. This eliminates the issue of data skew as data will be clustered with consistent file sizes.

On a delta table liquid clustering is used as described above to group small files together in a query-optimized way. In pipelines it is enabled directly on a streaming table with a cluster by  clause. 
You can use CLUSTER BY AUTO to let databricks learn and evolved with query patterns. When this is first done the table properties will show up as clusterByAuto=true,clusteringColumns=[] as databricks needs some time to determine which columns to use.
Alternatively you can select specific columns if you want full control and have a stable table with well-known query patterns. 
For instance a regulatory or performance-critical table.
You can use a hybrid approach and set columns initially but also enabling clusterByAuto.
```
CREATE OR REFRESH STREAMING TABLE my_table
CLUSTER BY (region, order_date)
AS SELECT * FROM STREAM source_table;
```
