
In structured streaming, data sources are expected to be append only, so changes in existing data including updates and deletes are not allowed. 

One option is to ignore deletes and changes. set ignoreDeletes to true, skipChangeCommits to true which also includes deletes. This works if it's possible to add separate logic to handle changes if needed.

The other options is CDF which allows tracking of row-level changes between versions of a delta table. It is a log of changes at row level from streaming tables to use in downstream streaming applications such as audit logs or CDC propagation.
It can sequence by delta version or or by last processed timestamp. CDF must be manually enabled sung delta.enableChangeDataFeed = true. it is slightly more storage intensive due to CDC metadata.
It can then be consumed as either stream or batch.
 With stream CDF is processed as each source commit completes. With rapid source commits you can have multiple delta versions in a single micro batch and need handling to choose sequencing order.
 You can consume using the change_data folder or the table_changes(table_str, start , end) function. it returns the data with change_type (insert, delete, update_preimage for previous value,  update_postimage for updated value), commit_version, commit_timestamp. If you set a delta version that doesn't exist yet as the end e.g. `table_changes('silver_users', 2, 3)` when 3 doesn't exist you will get an out-of-range error.

CDF vs CDC
CDF only delta lake tables. enabled on delta tables to process only changed rows for operations within data bricks. Uses the `_change_data` folder and the `table_changes` function.
CDC General concept, any system. captures data changes from external sources to sync incremental changes from source databases. Implemented via the Apply Changes syntax

Data deletion requests can be streamlined with automated triggers using structured streaming. CDF can identify records to be deleted in downstream tables but deleted values are still present in older versions of the data.
File retention policies are not applied until VACUUM is applied so do that regularly. Remember that VACUUM deletes CDF data. To run a VACUUM with less than 7 days of retention you will need to set retentionDurationCheck.enable = false.

You can add comments to delta commits either globally with `spark.databricks.delta.commitInfo.userMetadata` = `some string` or at the notebook level with `.option('userMetadata', 'some string 2')`. If both are set the more specific notebook level comment takes precedence.