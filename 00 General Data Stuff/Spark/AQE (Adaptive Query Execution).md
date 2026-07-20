
AQE is the runtime adaptability to optimize execution based on runtime stats and actual data. It adjusts the execution plan and during runtime can do dynamic partition pruning, join re-ordering, and other optimizations.

Available in Spark 3.0+
Enabled by default
Data considered skewed if >256MB and 5x large than average partition
skew can't be detected if your job has more than 2,000 shuffle partitions.
![[Adaptive Query Execution.png]]