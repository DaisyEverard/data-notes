### RDDs
`getNumPartitions()` - returns number of partitions

`map` - tranform a row -  `my_rdd.map(lambda row: (row[0], row[1]*10, str(row[2])))`

`filter` - `my_rdd.filter(lambda row: row["score"] > 80)`
`foreach` - run lambda on each element, no return value

`take` - get the top x results like limit with table reads. `my_rdd.take(5)`

`reduce` - reduce all values to a single value using a lambda.
values is the column to reduce, lit(0.0) is the starting value, acc is the rolling total or 'accumulator', x is the new row.
If you want to target a single column you can apply a map first to filter just to that then do the reduce.
It can only be used for processes which are both [[Glossary#^cd7ae8|associative and commutative]], for example addition and multiplication but not division or subtraction due to spark's parallel processing.
```
df = spark.createDataFrame([(1, [20.0, 4.0, 2.0, 6.0, 10.0])], ("id", "values"))
df.select(reduce("values", lit(0.0), lambda acc, x: acc + x).alias("sum")).show()
```

`glom` - shows you how data is partitioned. short for conglomerate as it gathers all items from each partition into a list. It is used in this example to show that reduce should only be used on associative and commutative processes.
![[glom example.png]]
`toDF` - convert an RDD to a dataframe and provide column names `my_rdd.toDF(["col1", "col2])`
### SparkContext
`broadcast` - use to make data the every node needs to be able to do processing. For instance a dictionary with a key value pair of short and long version of codes which may turn up on any node
```
broadcastCodes = spark.sparkContext.broadcast(codesDictionary)
result = my_rdd.map(lambda x: broadcast_var.value[x["short_code"]])
```

`accumulator` - this intialises a variable to keep track of something. takes argument of intial value
```
true_count = spark.sparkContext.accumulator(0)
remaining_lives = spark.sparkContext.accumulator(3)

true.add(3)
remaining_lives.subtract(1)
```

### DataFrames
`df.show()` - takes int argument for how many rows to show and truncate as a boolean
`df.rdd` - returns the underlying RDD of the dataframe. property, not method
`df.printSchema()` - print column names, dtypes, and nullable

**spark.read()** - for reading in external datasets. Many options as defined [here](https://spark.apache.org/docs/latest/sql-data-sources-csv.html).
useful ones include header (true/false) and inferSchema(true/false)
When reading in a csv with the .csv('path') option make sure to set the option('delimiter', ' ') if the delimiter is not comma.

`spark.write()` - you can provide a path to a pre-defined method for some formats e.g. write.csv(path) or write.parquet(path)

`.describe()` - returns a new databrame with counts, means, stddevs, mins, and maxs for data analysis
`.drop()` - drop columns, if multiple use comma separated list
`.withColumnRenamed(old name, new name)` - return dataframe with a column renamed. You can also replace series with `dataframe['new'] = dataframe['old']; dataframe.drop('old')` but it's not recommended

### Dataframe Analysis
`.filter(col('col') == value)`  - using pyspark.sql.functions
`.filter(df.col > value)` - without using sql function plugin
`.select(['col1', 'col2'])`
`.orderBy('col1', ascending=False)`
`.groupBy('col1').sum()`


SparkSession.sql() - Can only be used if a dataframe is stored as a local temporary view with df.createOrReplaceTempView('name')
