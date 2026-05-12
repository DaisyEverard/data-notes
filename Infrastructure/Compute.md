### Interactive Clusters
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
If a workload is time-sensitive turn on for faster startup but higher costs
### SQL Warehouse
- for SQL queries, dashboards, and BI
- High concurrency + autoscaling for low latency
- auto-start/auto-stop
- adjustable cluster size, can add a max