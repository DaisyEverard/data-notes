Spark is used for distributed compute of big data.

the **Spark Driver** - used as the entry point and is used to create a spark session. communicates with the cluster manager to created RDDs (resilient distributed datasets)

**RDDs** - created by dividing data randomly and distributing across worker nodes in a cluster with duplication for fault tolerance.

**Transformations**  - manipulate RDDs on a cluster
**Actions** - return a computation back to the main driver program
![[Spark diagram.png]]
### Processing
Jobs are run in parallel. Each is broken down into stages which are ordered steps. Stages are broken down into tasks where a partition of data to process is assigned

The Driver controls Worker nodes. Worker nodes host executor processes. Executors how the chunk of data to be processed. The chunk of data is the spark partition.
Executors have multiple slots/cores/threads which can each be assigned a task.

### Modules
modules have been developed to add extra functionality to spark. Some examples are:
**SparkSQL** - converts SQL into spark tasks

**Stark Streaming** - for processing live data streams, creates a dicretized stream (Dstream) of RDD batches

**MLlib and ML** - machine learning for pipelines which work with ML algorithms. ML is a dataframe based improvement of the original MLlib module

**GraphX** - visualizing data, coverts RDDs to RDPGs (resilient distributed property graphs) which utilize vertex and edge properties for relational data analysis

### Issues
Expensive - time efficient but not cost effective. needs a lot of RAM which is more expensive than disk memory

Real-time processing not possible - near real time works but true real-time doesn't

Manual Optimisation - You need a dev with an in-depth knowledge of the program and spark implementation to be able to do optimization of spark processing.