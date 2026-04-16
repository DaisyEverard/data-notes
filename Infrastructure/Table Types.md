### Streaming tables
Registered to Unity Catalog.
Pipeline is automatically generated for it.
Can be used for incremental data loading from kafka

--------------

## Managed tables

Databricks managed table and metadata
Data stored within Databrick's managed storage
Dropping the table also deleted the table
Recommended for creating new tables
## External Tables
Databricks only manages the table metadata
Dropping the table does not delete the data
Supports multiple formats including delta lake
Ideal for sharing data across platforms or using existing external data.