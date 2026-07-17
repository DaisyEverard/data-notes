Fan out - one source to multiple sinks
funnel - multiple sources to one sink

### The Quarantine Pattern
All data goes through one quality gate. There are then 2 paths for processing, one for data that passed evaluation and one for data that fails evaluation.
This means no record is ever dropped and Total records in = Clean records + Quarantine records. It also improves read performance when diagnosing errors as you read only a failed data table rather than all data with a filter for only failed.


To achieve this in Databricks use WARN as your constraint level rather than DROP ROW or FAIL UPDATE