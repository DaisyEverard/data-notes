Avoid external orchestration and orchestrates workflows in a way that is unified with your lakehouse.

Can orchestrate data, analytics, and AI workloads using different task types.

Trigger types:
- Scheduled - specified with cron
- Manual
- Continuous - always on. for streaming.
- File Arrival - support databricks volumes, AWS S3, Azure, GCP
- Table Updates - you can specify multiple tables and decide if the job runs when any is updated or all. You can also set a "minimum time between triggers" of the job and a delay after table update with "wait after last change"
- API Trigger

ties in with Observability for montioring and troubleshooting, Control Flow for managing tast dependencies and execution order.

Langauges supported: Scala, R, python, SQL, Java (via JAR file)

Job Types:
- notebook - run a notebook
- SQL - sql query file
- for each - The whole for each is one container and registers as one job, it has a nested task that runs for each iteration. loops over an `{{[<input>]}}` array, runs nested task for each one.
- if/else - boolean logic. Used for things like record counts, null percentages, validation results etc. The options are job and task parameter parameters and values. You can set different paths for true and false, it's not just a simple pass/fail
- Dashboard ask


## Configuration
#### **parameters and dynamic value reference:**
can only be set at task level, if a key at job level is the same as at task level the job level one takes precedence. Job parameters take defaults but can be overwritten at runtime. Both fetched by `dbutils.widgets.get()`

to pass info between tasks use task values
`dbutils.jobs.taskValues.set(key="foo", value="bah")`
`dbutils.jobs.taskValues.get(taskKey="<task-name>", key="foo")`

dynamic value references are calculated automatically but change on runtime. They use `{{}}` notification for instance `{{task.name}}` or `{{job.start_time.day}}`.
For a full list see: https://docs.databricks.com/aws/en/jobs/dynamic-value-references

#### **conditional task dependencies**
based on if past jobs succeeded or failed in whatever combination you need. They will wait for all tasks to complete before continuing.
- all succeeded
- At least one succeeded
- None Failed - diffferent to all succeded as it also allows skipped tasks
- All done - ignore conditions, this exists because the default when not set is 'all succeeded'
- At least one failed
- All failed

#### **retries**
 Task or job level. Can configure number of retries and time between retries.

