Spill is moving data from RAM to disk then back into RAM. It happens when a partition is too big to fit into the RAM of a cluster. It leads to expensive disk reads and writes to avoid the OOM error.

It can be caused my many things but common ones include:
- `spark.sql.files.maxPartitionBytes` is too high
- `spark.sql.shuffle.partitions` too low or repartition() used wrong.
- join() or cross() join of tables with skewed key or lots of new rows to generate
- groupBy() with low cardinality column
- countDistinct(), explode(), size(collect_set())

Spark surfaces 2 kinds of spill. spill (Memory) is the size of data that existed in memory. Spill (Disk) si the size of the data on disk.