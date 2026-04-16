### RDDs
`getNumPartitions()` - returns number of partitions

`map` - tranform a row -  `my_rdd.map(lambda row: (row[0], row[1]*10, str(row[2])))`

`filter` - `my_rdd.filter(lambda row: row["score"] > 80)`

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

`broadcast` - use for 