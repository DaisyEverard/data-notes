#### Partitioning
Databricks recommends [[liquid clustering]] for new tables.
Don't over partition. Only partition where there are logical groups or you need a physical boundary for data such as SCD Type 2 or GDPR reasons. Partition size should be between 1GB and 1TB for large tables.
Don't partition on columns where every row will be distinct like ID, partition into sensible groups like region, consider data skew in these groups. The best columns to partition on are ones often filtered on.
You can repartition data into n partitions manually using `.repartition(n)`

#### Data Skipping
A pruning technique to reduce the number of irrelevent files searched. It uses file-level stats like min & max

#### Z-ordering
is a way to organize data based on one column so similar values are stored together in the same files. min and max for that column will be in each file's metadata. Liquid clustering is preferred to z-ordering for new tables.

#### File metadata
Databricks collects stats about the first n columns. n set with `dataSkippingNumIndexedCols = n`
The metadata stats are used in some queries, for instance `SELECT MAX(col) FOO foo` does not actually open any files if there are stats for the column queried. This doesn't work well on TimeStamp and String columns as truncation can prevent exact matches.
Filters are applied in the order of: partition filters, data filters, pushed filters.

#### Other
table statistics can be computed using `ANALYZE TABLE table COMPUTE STATISTICS FOR ALL COLUMNS` to support optimization on how files are read and joined. Especially useful combined with Adaptive Query Execution.

Predictive optimization enabled in databricks allows continuous monitoring of workload and auto-optimization configurations and processes. It specifically automates execution of background maintenance tasks and supports OPTIMIZE to retune file sizes and VACCUM to reduce storage costs of unused data. It uses serverless compute.