skew is when data is unevenly partitioned meaning some nodes are overloaded while others are idle. It causes performance bottlenecks and means your resources are not being used optimally. Data can be skewed on read or become skewed via transformations.

for instance aggregating by city when one city is twice as big as any other. This will mean the one big city's partition will take twice as long to process as the rest, delaying the job finishing and potentially not having enough RAM.

strategies include:
- [[AQE (Adaptive Query Execution)]] (preferred in databricks)
- filtering - For when you can remove the skewed data, for instance a lot of null values that can be filtered out
- skew hints - If you know the table, column, and values causing skew you can explicity tell stark so it can try to resolve it
- salting - break down large partition into smaller partitions by appending random ints as suffixes.
- broadcast joins, repartitioning (optionally with salting), adjusting shuffle partitions, or separately handing heavily overloaded keys and processing differently if it's known what they are.

In spark this isn't much of a concern due to 