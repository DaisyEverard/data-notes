Apache Spark™ Declarative Pipelines is a declarative framework for building batch and streaming data pipelines in SQL and Python. Lakeflow Declarative Pipelines extend and are interoperable with Spark Declarative Pipelines, while running on the performance-optimized Databricks Runtime.

The pipelines automatically handle execution plans, error handling, and dependency management. They scale automatically and adapt to optimize for performance and cost efficiency. There is a dry-run capability to check pipeline without full execution.

Lets you define ingestion and transformation tasks with SQL and Python without explicit orchestration logic. Allows Batch ingestion or Streaming ingestion 

Streaming will use checkpoints to track what data has already been processed.

You can have different settings on your [[Compute]] 
Classic (fixed size):
Users need permission to create computer for pipelines
Workspace admins can configure cluster policies
Autoscaling:
enabled by default. Optimizes cluster utilization.

In the pipeline source code settings you can give a 'pipeline root folder' containing multiple code files, including git folders to include all contents. The 'source code' section lets you select a subfolder or individual file to include.

Parameters set in the configuration e.g. `source: "<source>"` are referenced in sql with the syntax`${source}`

You can use the `Publish event log to metastore` toggle to publish the event log as a publish delta table, you can also customize the name, catalog, and schema. You can query this table for reports, analysis, and dashboards. By default this is off and event logs are a hidden table in the pipeline's default schema.

### Expectations
Expectations are rules to validate your data. 
	`CONSTRAINT constraint_name EXPECT column != 'foo' ON VIOLATION <action>`
The actions are
WARN to log a warning but still write the invalid row. count of invalid rows is logged
DROP to drop the invalid row. Counts of dropped rows are logged
FAIL fails the entire flow, other flows in the same pipeline continue.

If a materialized view uses expectations it will always be fully refreshed during pipeline runs.
You can check the pipeline UI or system tables for metrics on expectations. You can find out the number of records processed, how many records fail each constraint, the violation percentage, and the historical trends of other quality metrics of time.

For more complex scenarios you will need to use advanced quality expectations. In practice this is achieved by chaining conditions and logic.
Basic expectations have NOT NULL but there are many thing dies doesn't catch:
- numeric anomaly - negative order quantity
- termporal inconsistency - event date set to 50 years ago as default
- range violation - 120% discount rate
- optional field rules - field can be null but not if present must be >0
- schema evolution - new columns added mid-stream break existing rules
- Data loss - invalid records permanently dropped with no audit trail
- 
Check no rows were dropped between two tables:
```
CREATE OR REFRESH MATERIALIZED VIEW count_verification (
  CONSTRAINT no_rows_dropped EXPECT (a_count == b_count)
    ON VIOLATION FAIL UPDATE
)
AS SELECT * FROM
  (SELECT COUNT(*) AS a_count FROM table_a),
  (SELECT COUNT(*) AS b_count FROM table_b)
```

Check there are no records missing, for instance from a dim table.
```
CREATE OR REFRESH MATERIALIZED VIEW report_compare_tests (
  CONSTRAINT no_missing_records EXPECT (d_key IS NOT NULL)
    ON VIOLATION FAIL UPDATE
)
AS SELECT v.*, d.key AS d_key
FROM vehicle_sales v
LEFT OUTER JOIN dim d ON v.key = d.key
```

Primary Key uniqueness
```
CREATE OR REFRESH MATERIALIZED VIEW report_pk_tests (
  CONSTRAINT unique_pk EXPECT (num_entries = 1)
    ON VIOLATION FAIL UPDATE
)
AS SELECT pk, COUNT(*) AS num_entries
FROM report
GROUP BY pk
```
### [[CDC and SCD]]
https://docs.databricks.com/aws/en/ldp/developer/ldp-sql-ref-apply-changes-into
There is a statement in pipelines called `AUTO CDC INTO` to handle upserts, insert, and deletes all in one.
from and keys are enough for inserts and upserts, for deletes you need to specify when a delete happens, for instance there may be a column with a DELETE string or boolean flag.
SEQUENCE BY defines the logical order of cdc events in the source
COLUMNS states which columns from the source to include in the target as they may not be the same shape
	STORED AS allows you to use TYPE 1 (delete historical records) or TYPE 2 (retain historical records with validity periods). Default is type 1. If you do type 2 you will also need a TRACK HISTORY ON clause to specify which columns to track the history of. if you don't specify a TRACK HISTORY ON clause all columns are treated as on group and  `__START_AT` and `__END_AT` columns are generated
```
AUTO CDC INTO target
FROM STREAM source
KEYS (key1, key2)
APPLY AS DELETE WHEN operation_col = 'DELETE'
SEQUENCE BY ProcessDate
COLUMNS * EXCEPT (operation_col, irrelevant_col)
STORED AS SCD TYPE 1
```

## Flows
https://docs.databricks.com/aws/en/ldp/flows
A flow is a simple unit of work which independently read, processed, and writes data. At it's simplest it could be a query plus a target. 
They can be incremental with read checkpoints targeting and processed only the delta, or do a full refresh with a discard checkpoint then read and reprocess all records. This should be used after modifying transformation logic or fixing data quality issues.

Flows maintain their own checkpoints.
- Flow Name = checkpoint identity - The name of a flow is also its checkpoint location in storage. 
- Renaming Resets progress - A new name will mean the old checkpoint with the old name will be abandoned and the flow will have to reprocess all data to create a new checkpoint with the new name
- Independent Progression - Flows do not affect other flow's processing
- Failure Isolation - One failed flow does not fail others.

Flows are usually created automatically and implicitly when a streaming table or materialized view is created. The default flow will share the target table's name. for instance:
```
CREATE OR REFRESH STREAMING TABLE target_table
AS SELECT *
FROM STREAM source_table;
```
You can create flows explicitly if you need to write to an existing table from a new source:
```
CREATE OR REFRESH STREAMING TABLE target_table;
CREATE FLOW my_flow
AS INSERT INTO target_table BY NAME
SELECT * FROM STREAM source_table;
```

A multi-flow pattern is when multiple different flows target the same table. 
This is better than using a union pattern for incremental pipelines because of independent checkpoints meaning new sources can be added with a full refresh, one source failure doesn't block others, the lineage; error handling and monitoring is cleaner, and it's more scalable and maintainable.

Data quality expectations using CONSTRAINT cannot be defined on a single flow, they must be defined on the target table. You can query the pipeline event log which have one row per flow update. for instance:
```SELECT timestamp, table_name, output_rows,
       data_quality.expectations
FROM event_log("pipeline_id")
WHERE event_type = 'flow_progress'
  AND data_quality.expectations IS NOT NULL
ORDER BY timestamp DESC;
```

## Liquid Clustering
A data optimization technique for Delta lake. Replaces traditional partitioning and z-ordering instead using clustering keys to efficiently skip data.
Optimizes files for performance during pipeline runs. Uses a hilbert curve to make file boundaries tighter. I've added a picture of a hilbert curve because maths is cool.
![[hilbert.gif]]
Hive partitioning used a rigid directory structure requiring a full rewrite to change the keys. Z-ordering required manual OPTIMIZE runs with a full rewrite on every run. 

Liquid clustering is:
- Incremental - only optimizes new or unclustered data so works for streaming and write-heavy workloads
- Flexible - clustering keys can be updated at any time without full table rewrites. They can adapt to evolving query patterns
- Self-Tuning - with CLUSTER BY AUTO databricks will select optimal keys based on query usage.

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

## Multiplex Pattern
A way of efficiently processing multiple event types that arrive through a single data transport mechanism such as a kafka topic, cloud storage path, or message queue.

Multiplex means you can ingest all the different event types in one ingestion pipeline with 1 checkpoint and 1 source scan shared across all the events. the reduced the maintenance and overhead in creating a separate pipeline for every event type.
1. Everything is ingested into one bronze table
2. Filter on the event type field to separate domains
3. Fan out to domain specific tables and processing downstream

## Sinks
Sinks  provide a way to write streaming data from spark pipelines to any plain delta table outside of a pipeline's managed scope.
You can only use the python API, not SQL.
`append_flow` is the method used to write to a sink. It automatically handles checkpoints and only writes the delta

Managed tables have data that stays in UC, there's full pipeline lineage tracking, expectations and CDC are supported, and you can create streaming tables and Materialized views.
Sinks write to external systems outside databricks so are append only and can't support expectations. This however enables reverse ETL, operational use cases, and Iceberg Uniform. It supports Kafka, Event Hubs, and custom python sinks

To define and write to sinks: 
```
from pyspark import pipelines as dp
dp.create_sink(
    name    = "my_sink",
    format  = "delta",
    options = {
        "tableName": "catalog.schema.table"
    }
)
```

```
@dp.append_flow(
name   = "my_sink_flow",
target = "my_sink"
)
def my_sink_flow():
    return spark.readStream.table(
        "schema.source_table"
    )
```

## Iceberg Reads + Delta UniForm
Delta UniForm enables cross-platform access to Delta tables by automatically generating Apache Iceberg metadata without duplication the underlying data. 
This means you don't need multiple copies of the same data in different formats to support multiple platforms. This creates read only iceberg sources, writes must got through delta. 

There is one set of parquet files with delta and iceberg metadata layers. The delta Transaction log `_delta_log/` and the Iceberg metadata at `metadata/*.metadata.json`. Unity catalog acts as the Iceberg REST catalog.


The pre-requisites for iceberg reads are:
- Must be on a plain external delta table e.g. one created with a delta sink. streaming tables and materialized views cannot have iceberg reads enabled. 
- The table has no data written to it yet
- disable deletion vectors as iceberg cannot represent soft deletes. 
  `'delta.enableDeletionVectors' = 'false'`
- Column identifiers need to be consistent between delta and iceberg schemas. `'delta.columnMapping.mode' = 'name'`
- IcebergeCompatV2" enabled to activate delta's write protocol for iceberg `'delta.enableIcebergCompatV2'='true'`
- universal format enabled to trigger async iceberg metadata generation after each delta commit. Generation is only triggered when the table is accessed by name, not by path.
  `'delta.universalFormat.enabledFormats'='iceberg'`

![[Pasted image 20260716091845.png]]
## Other
Change Data Feed (CDF) - capture row level changes from streaming tables and use them in downstream applications, such as audit logs or CDC propagation