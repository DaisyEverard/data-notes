Shuffling is moving data from the output of one stage to the input of another. This means redistributing and reorganizing data across nodes so requires data movement and can slow down processing.
It is usually a side effect of wide transformations.
![[MapReduce.png]]

###### Narrow data transformations
Each piece of input data contributes only one part of the output. This means the data required to process the records in one partition comes from at most one partition of the parent RDD.
Most importantly they do not require shuffling. Some examples include Map and Filter
###### Wide data transformations
input data from multiple partitions contribute to a single partition in the output. This is more common and requires shuffling of data across multiple partitions.
examples include GroupBy and ReduceByKey transformations.

Reduce the amount of shuffling you have to do by:
- using fewer, larger worker nodes
- use NVMe data transfers (see [[Glossary]])
- remove unnecessary columns and records
- denormalize datasets, especially for join related shuffles
