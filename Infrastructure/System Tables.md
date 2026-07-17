#### system.lakeflow
read-only catalog, logs job activity for all workspaces in a region
timeline tables slice long runs hourly using period_start_time, period_end_time

Useful for analytics on cost, performance, SLA compliance, and resource utilization

tables:
- jobs - basic job info
- job_tasks - task basic definitions
- job_run_timeline - each job run over time
- job_task_run_timeline - each task run over time
- Pipelines - pipelines basic info
https://docs.databricks.com/aws/en/admin/system-tables/jobs

#### Object metadata - system.information_schema
list tables in a catalog or schema with `system.information_schema.tables`
Find who has what privileges on a table with `system.information_schema.table_privileges`
Get owner, table name, who and when it was later altered from `system.information_schema.tables`

tags applied on columns are queries with `information_schema.column_tags`, other tables exist for catalog_tags, table_tags, and schema_tags.
https://docs.databricks.com/aws/en/admin/system-tables/index.html

#### Billing Logs - system.billing
https://docs.databricks.com/en/admin/system-tables/billing.html
`system.billing.usage`
break down DBU (Databricks Unit) usage per date, job, user, or SKU (Stock keeping unit)
SKU means the category of product you use such as compute type, table, model serving, or cloud VM SKUs.

#### Audit Logs - system.access.audit
For questions about which table is accessed by most or who accesses a table the most. Also record identities with deletes and other table operations.
https://docs.databricks.com/en/admin/system-tables/audit-logs.html

#### Lineage - system.access.table_lineage
Where tables source data from and what user queries read from the table
https://docs.databricks.com/en/admin/system-tables/lineage.html