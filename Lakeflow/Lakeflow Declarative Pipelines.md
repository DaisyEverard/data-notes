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

### Expectations
Expectations are rules to validate your data. 
	`CONSTRAINT constraint_name EXPECT column != 'foo' ON VIOLATION <action>`
The actions are
WARN to log a warning but still write the invalid row. count of invalid rows is logged
DROP to drop the invalid row. Counts of dropped rows are logged
FAIL fails the entire flow, other flows in the same pipeline continue.

If a materialized view uses expectations it will always be fully refreshed during pipeline runs.