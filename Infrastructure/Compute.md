### Interactive/All-Purpose Clusters
aka all-purpose
- can be shared by multiple users but this can cause conflicts and job delays.
- for ad-hoc analysis, data exploration, or dev
- Do not use in Prod, not cost efficient and limited scalability. 

### Job Clusters
- 50% cheaper than Interactive because they Terminate when job ends.
- Subject to cloud provider startup times
- can re-use the same cluster across tasks in one job
- Each job gets dedicated resources for predictable performance but requires more configuration and management overhead
### Serverless
- fully managed
- Faster clusters
- auto scaling
- no cost-of-ownership

Has a performance optimized toggle
off is standard and focuses on cost-efficiency with a longer startup time
If a workload is time-sensitive turn on for faster start-up but higher costs
Supports incremental refresh of materialized views using a cost-based optimizer
### SQL Warehouse
- for SQL queries, dashboards, and BI
- High concurrency + autoscaling for low latency
- auto-start/auto-stop
- adjustable cluster size, can add a max

## Spot Instances
Low cost compute which is currently unused but can be reclaimed by the provider at any time if demand increases. Good for ad-hoc and shared clusters but not for anything that needs reliability.
Never use them for a driver.

----
## Photon
query engine. Very fast and no code changes needed. Supports SQL, Python, Scala, R, and Java

## Instances
For first run set `spark.sql.shuffle.partitions = 2x # of cores`
Total memory available should be less than 128gb
1 core to approximately 128mb-2gb of reads.
Check there is no spill. If there is try setting the shuffle partitions to auto/increase number and increasing instance size

For specific requirements, make sure your instance family is compatible with photon, joins/windows/groupBys/aggregations,
If you want to use photon make sure you instance family is compatible