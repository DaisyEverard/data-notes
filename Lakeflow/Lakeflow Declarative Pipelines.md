Pipelines that automatically handle execution plans, error handling, and dependency management
They scale automatically and adapt to optimize for performance and cost efficiency. 

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

### [[CDC and SCD]]
https://docs.databricks.com/aws/en/ldp/developer/ldp-sql-ref-apply-changes-into
There is a statement in pipelines called `AUTO CDC INTO` to handle upserts, insert, and deletes all in one.
from and keys are enough for inserts and upserts, for deletes you need to specify when a delete happens, for instance there may be a column with a DELETE string of boolean flag.
SEQUENCE BY defines the logical order of cdc events in the source
COLUMNS states which columns from the source to include in the target as they may not be the same shape
STORED AS allows you to use TYPE 1 (delete historical records) or TYPE 2 (retain historical records with validity periods). Default is type 1. If you do type 2 you will also need a TRACK HISTORY ON clause to specify which columns to track the history of.
```
AUTO CDC INTO target
FROM STREAM source
KEYS (key1, key2)
APPLY AS DELETE WHEN operation_col = 'DELETE'
SEQUENCE BY ProcessDate
COLUMNS * EXCEPT (operation)
STORED AS SCD TYPE 1
```

### Other

Flows
multiple flows writing to the same target. Union of different data sources from multiple pipelines. https://docs.databricks.com/aws/en/ldp/flows

Sinks
write pipeline outputs to external systems. Kafka, Delta across workspaces, or Azure Event-Hubs

Liquid Clustering - Optimizes files for performance during pipeline runs

Row-Level Security and Column masking - fine grained access control for streaming and materialized tables

Change Data Feed (CDF) - capture row level changes from streaming tables and use them in downstream applications, such as audit logs or CDC propagation

Databricks Asset Bundles (DABs) - enable you to programatically validate, deploy, and run Databricks resources such as pipelines for CI/CD prod workloads.