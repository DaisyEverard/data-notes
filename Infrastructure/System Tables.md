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
