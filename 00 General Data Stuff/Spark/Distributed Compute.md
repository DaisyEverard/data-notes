Distributed file systems are a storage system which use clusters.

Clusters are groups of node, these can be real machines or VMs
In each cluster there are many worker nodes with storage and one cluster manager node with just compute to manage the rest.
Data is duplicated on multiple worker nodes for fault tolerance. This kind of system is infinitely scalable.

### HDFS
Hadoop Distributed File System is popular cluster system framework by Apache. It's designed to be used with **MapReduce**, a data processing framework.
The negative is that it requires a specific hardware configuration with high upfront setup cost for on-prem so this is almost always used with cloud storage solutions.

### Object Storage
A type of distributed file system that separates storage from compute instead of having both on all worker nodes. It provides only a storage layer which you can put any other computing on top of.
They are more flexible in that you can store any kind of file in any format so you can use high performance formats like Delta and iceberg. You can also more efficiently adjust your required amounts of storage and compute instead of having to scale them at the same ratio.


### MapReduce
Now Apache Spark is used a better alternative.
MapReduce is a framework composed of 2 actions: map and reduce.
Map collects specifically defined elements of data from each node ad key-value tuple pairs
Reduce is an analytical function applied to each tuple.
The advantage is that all nodes can work independently without bottlenecks where they are waiting on other processes.

### Apache Spark
better performance big data processing system than MapReduce due to being able to process data in a worker node's memory instead of processing on disk like MapReduce
up to 100x faster as it caches data and intermediate tables in RAM however the advantage decrease or even disappears on bigger datasets.
It can be run on a single node/computer so isn't necessarily distributed compute but is usually.

### Data Partitioning
How data is split up and sent to different nodes. The default in spark is 200 partitions.
##### Narrow data transformations
Each piece of input data contributes only one part of the output. This means the data required to process the records in one partition comes from at most one partition of the parent RDD.
Most importantly they do not require shuffling. Some examples include Map and Filter
###### Wide data transformations
input data from multiple partitions contribute to a single partition in the output. This is more common and requires shuffling of data across multiple partitions.
examples include GroupBy and ReduceByKey transformations.

###### execution plans
a two stage process of logical and physical plans.
The logical plan is an abstract representation of the sequence of steps required. Spark's catalyst optimizer analyses and applies optimization rules.
The physical plan is how the query will be executed in the spark cluster including partitioning, shuffling, and aggregation across nodes.
