You can create a new session with:
```from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate() 

# end with
spark.stop()

```
The default is to use the number of CPU cores on a local machine as the number of partitions.

`sparkContext` is the connection to the cluster which is used to create and transform RDDs.
for locally stored data use `parallelize`

```
rdd_par = spark.sparkContext.parallelize(dataset_name)
```
For external or large data on distributed file systems use different methods e.g. `textFile()`. The default partition for this is 128MB chunks but, like parallelize, you can add a parameter for the number of partitions
```
rdd_txt = spark.sparkContext.textFile("file_name.txt", 10)
```

