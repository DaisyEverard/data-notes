### Metadata
information can be appended using the hidden `_metadata` column.
It includes `_metadata.file_modification_time` and `_metadata.file_name`

The full list can be found here: https://docs.databricks.com/aws/en/ingestion/file-metadata-column

You manually add these column with  `select _metadata.property AS x` as you would with any aliased column

### Rescued Data
captures mismatched or unparseable fields as json when you use `read_files`, `spark.read` or `Auto Loader`

e.g Trying to put a string in a numerical column with result in the `_rescued_data` for that row being `{"numerical_column_name": "string_value", "_file_path": "path"}`
If you try to put a number in a string column that will not fail and rescued data will be empty

You include it in a .read by using `.option("rescuedDataColumn", "_rescued_data")`

### Schema Hints
schemaHints are a way to declare a new column which is expected to appear in upcoming files but isn't in the schema yet. It will populate automatically once records with the new column come through.
Old rows will have NULL for the new column, so any constraints on the column must be null-tolerant to not drop all the old data.
`FROM STREAM read_files(...schemaHints => 'loyalty_tier' STRING, region_code STRING)`

