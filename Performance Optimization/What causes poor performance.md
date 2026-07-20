Best practice includes:
- use dataframes or SQL instead of RDDs
- Avoid operations other than read or write where possible. For instance count(), display(), collect()
- Avoid forcing all computation into the driver node such as single threaded python/pandas.

Performace considerations include:
- **Bytes read** 
more data = slower

- **Query complexity**
more computation from aggregations, joins etc.

- **number of source files**
Too many small files or file system blocks increase time open, read, and close, and deal with metadata. Databricks automatically tunes size of delta lake tables and compacts small files on write with auto-optimize with the goal of standarized 124MB files. Conversely, too few large files reduces parallelism so you need a balance.
  
- **Parallelism**
more threads in parallel means less real time.

- [[Skew]]
