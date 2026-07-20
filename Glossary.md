**Idempotency** - Tracking files which have previously been ingested and only taking the delta
**Cardinality** - low cardinality=few unique values. high cardinality means a lot of unique values. An ID column would be high cardinality.

[[RDDs]] - Resilient Distributed Datasets
RDPGs - resilient distributed property graphs
DAG - Directed Acyclic Graph. The way
NVMe - ==**Non-Volatile Memory Express**==. fast data transfer protocol designed specifically for modern flash storage (SSDs)
SSDs - Solid-State Drive. non-volatile data storage device used in computers.

**Commuative Process** - a process where order does not matter
**Associative Process** - a process where grouping does not matter.
simple addition is associative and commutative  `a+b+c=c+b+a= c+(b+a)`
multiplication is commutative and associative `abc = (cb)a = c(ba) `
subtraction is not associative or commutative` a-b-c != (c-b)-a != c-(b-a)`
division is not associative or commutative `a/b/c != (c/b)/a != c/(b/a)`
string concatenation is associative but not commutative. `f"{ab}{c}" = f"{a}{b}{c}" != f"{b}{c}{a}"` ^cd7ae8

non-SARGable - when a database can't use an index efficiently to narrow down rows.

**Apache Iceberg**
Apache Iceberg is ==an open-source, high-performance table format for massive analytic datasets stored in data lakes==. Developed by Netflix to solve data consistency and scalability issues, it acts as a structured abstraction layer that sits on top of physical data files (like Parquet or ORC) to bring the reliability and simplicity of SQL databases to large-scale data lakes.