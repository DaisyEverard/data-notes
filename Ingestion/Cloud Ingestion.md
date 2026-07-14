Convert raw formats like JSON, CSV, and Parquet to Delta Tables

# NEW TABLES
### CTAS
Batch only, for small datasets, high latency
Not idempotent
`CREATE TABLE AS SELECT * FROM read_files(
`<path to file>,`
`format = '<file type>',`
`...`
`)`

can detect file format automatically and infer schema as long as all files are the same schema
if you specify a format, only the files in that format from the directory will be read if it's mixed
can use autoloader for streaming

### COPY INTO
This is a legacy option and should not be used where possible. It is designed for incremental batch loads.
Only supported by SQL
```
CREATE TABLE X;

COPY INTO X
FROM '<directory path>'
FILEFORMAT => <file type>
FORMAT_OPTIONS(<options>)
COPY_OPTIONS(<options>)
```


### AUTO LOADER
Python or SQL. Uses Spark. Incremental Batch or Streaming.
Can read from volumes
```
spark.readStream
.format("cloudFiles.format", "json")
.option("cloudFiles.schemaLocation", "path")
.option(option2)
.load("path")

.writeStream
.option()
.trigger(once=true)
.toTable("catalog.schema.table")
```

In SQL this is equivalent to:
```
CREATE OR REFRESH STREAMING TABLE
catalog.schema.table SCHEDULE EVERY 1 HOUR
AS SELECT * FROM STREAM
read_files(
'path',
format => 'type'
)
```
- **format** e.g. "csv"
- **sep** e.g "|" for pipe delimited files
- **header** = boolean. If the file has a header, used for csv and similar types
- schema => ```"col1 TYPE, col2 TYPE"```
- for more see: https://docs.databricks.com/aws/en/sql/language-manual/functions/read_files


# EXISTING TABLES

use the [[MERGE INTO]] command to do updates, insertions, and deletions all in one query. It's great for slowly changing dimensions, incremental loads, and change data capture (cdc)

If you want to automatically add new columns from the source in the target table in databricks, you can use MERGE WITH SCHEMA EVOLUTION



