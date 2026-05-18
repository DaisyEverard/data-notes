
A view is a read-only object resulting from a query over other tables or views.

### Standard View
computes data every time you run it. low resource usage, high query time.
Registered object in UC.
Cannot contain streaming queries or be used as a streaming source
### materialized view
physically stores the precomputed results of a query, rather than recomputing it every time it is run. Used to improve query performance for complex joins or aggregation operations on large datasets.
Are recomputed when an update is processed using a refresh schedule or running a pipeline update. 
In serverless, Results are incrementally refreshed so you don't need a full rebuild on each update where possible. Whether they are fully recomputed or incrementally refreshed indecided by a cost-based optimizer.
`CREATE OR REFRESH MATERIALIZED VIEW`
You don't need the STREAM keyword in the from clause as this is automatically tracked.
Always fully refreshed during pipeline runs if it uses expectations

### Temporary View
not registered to schema or catalog, have limited scopes outside of which they are not accessible. 
Not registered to UC.
In notebooks and jobs they are scoped the the notebook or script. They no longer exist when the notebook detaches from the cluster.
In Databricks SQL they are scoped the the query. multiple statements within the same query can use the temp view but it can't be reference in other queries in the same dashboard.
Mostly used for intermediate queries that don't need to be exposed to end users
### Dynamic views
Used to provide row or column level access control and to mask data.

### Metric View
define reusable business metrics to abstract logic for KPIs like revenue, customer count etc. Used for consistency in business logic. Defined in yaml.


