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