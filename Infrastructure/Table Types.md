### Streaming tables
Registered to Unity Catalog. Pipeline is automatically generated for it. Can support batch data processing. Also for incremental data e.g. loading from kafka. New data is appended to the table.

The following is an example of creating a streaming table which uses a streaming read, meaning it will process new files as they arrive. read_files() returns data in tabular format.
```
CREATE OR REFRESH STREAMING TABLE t 
AS SELECT
*,
current_timestamp() AS processing_time,
_metadata.file_name AS source_file
FROM STREAM read_files("path", format => 'JSON')
```
You can also do `FROM STREAM < streaming table name>` if you're reading from another table. 

File names are guaranteed to be read only once so if you edit a file that has already been processed the changes with NOT be processed. 

--------------

## Managed tables

Databricks managed table and metadata
Data stored within Databrick's managed storage
Dropping the table also deletes the data
Recommended for creating new tables
## External Tables
Databricks only manages the table metadata
Dropping the table does not delete the data
Supports multiple formats including delta lake
Ideal for sharing data across platforms or using existing external data.

----

## Table Attributes

You can add tags to tables or to individual columns. For instance a PII tag to let users know where PII is stored
```
ALTER TABLE table
SET TAGS (
	'PII' = 'True',
	'Quality' = 'bronze'
)
```
You can see views by querying the <my_catalog>.information_schema.table_tags within a specific catalog, or system.information_schema.table_tags for all.