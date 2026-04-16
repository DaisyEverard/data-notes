There are 3 types of ingestion:
- manual file uploads
- standard connectors for sources like cloud object storage and Kafka. Support multiple ingestion modes.
- Managed connectors: for enterprise solutions including SaaS platforms. use incremental read/write.


### Lakeflow connect
- for external databases, not public APIs
- has an ingestion gateway with a pipeline that extracts metadata, snapshots, and changelogs
- stages data in a volume then reads from volume to table. access is limited to user running the pipeline by default

### Managed Connectors
Used to simplify ingestion from enterprise databases and apps. Fully managed by databricks and low-code.
A connector needs to fetch credentials from UC before connecting to an external service.

a `managed ingestion pipeline` is a type that connects to public SaaS sources to extract data and ingest to a streaming table, they are largely predefined and managed by databricks.. 

- for public APIs
- all movement happens in the data plane. The control plane is only for pipeline setup, monitoring, and management.
- reads directly to streaming delta table

Why is the extra gateway there? 
- helps with networking, firewalls, databases that aren't publicly accessible. The gateway can be deployed inside a network if you can't use private link.
- limits load on the database. number of direct connection can be one then fan out to multiple pipelines for scalability