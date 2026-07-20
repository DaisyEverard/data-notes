
#### Smells
**High planning time** ** suggests a need for better partitioning strategies or metadata optimization
**High execution time** suggests need for join optimization, broadcast strategies, or skew handling
**Resource Bottlenecks** suggests memory, CPU, or I/O constraints that require cluster configuration adjustments

### Partitioning Strategies
Partitioning is how spark splits data across cluster nodes, for more see [[Distributed Compute]]. 

Impacts of partitioning are: 
- **_Speed_** — More balanced partitions lead to faster execution.
- **_Scalability_** — Proper partitioning allows full CPU utilization.
- **_Memory usage_** — Oversized partitions can cause out-of-memory errors.
- **_Shuffle cost_** — Poor partitioning can trigger excessive and expensive network data transfers.

Strategies include 
Hash partitioning - default in spark. hash function of a key. Not suited for when some keys are more frequent.
Range Partitioning - based on key ranges instead of single keys. For when you need sorted data like some joins, or when you're preparing data for window functions.
Custom - you can create your own partitioner.

You can adjust dynamically with `repartition()` to increase the number of partitions or when data in unevenly distributed post-transformation.
`coalesce()` reduces number of partitions, use to avoid shuffling especially after filtering down a large dataset.
The default number of partitions in spark is 200.
### Metadata optimization
Structuring metadata to improve data discoverability and system performance.

In data bricks this is largely done automatically by enabling predictive optimization and liquid clustering. You can run the `VACUUM` sql command to manage table versions. It removed unreferenced data files past a retention period defaulting to 7 days. You will not be able to time travel after a VACUUM. 
log retention is json log.
data retentions is for the actual data files.
`OPTIMIZE` will compact small files.

You should generally avoid excessive partitioning.

### Join Optimization
filter in the join (not in the where clause after) to reduce amount of processed data.

 using something like `YEAR(o.OrderDate) = 2024` in the where means indexes can't be used efficiently. It's better to use something like `o.OrderDate >= 'x' AND o.orderData < 'y'`.
 This is because a YEAR function is applied to every for value before the condition can be evaluated. Index values are based on actual values so if you change the type or apply any transformation then the index needs an extra step to check YEAR(date) = date ?

 
 Use inner join instead of left if you filter out nulls later. for instance
 ```
 LEFT JOIN Payment pm ON pm.OrderID = o.OrderID  
WHERE pm.Amount IS NOT NULL
 ```
is the same as
```
INNER JOIN Payment pm ON pm.OrderID = o.OrderID  
```


Like condition `LIKE '%@example'` is less efficient than '\_ \_ \_ \_@example' which is less efficient than 'example%'. This is because indexes are more efficient when they can use the characters at the start of the value. an Underscore in a regex means exactly one character.

in general, Always try to join small to large rather than large to small to reduce movement of data.
### Broadcast Strategies
A broadcast join makes it easier for spark to join a large dataset with a smaller one so is good for lookup data or adding metadata.
A broadcast, instead of shuffling data between both tables, replicates a copy of the small table to all nodes in the cluster.
It is only suitable with joining on small datasets due to memory overhead issues. It also requires manual configuration so requires extra code and knowledge.

A simple example: 
`joined_df = large_table.join(F.broadcast(small_table), "relation_key")`

### Cluster Configuration
