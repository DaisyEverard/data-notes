Used to simplify ingestion from enterprise databases and apps. Fully managed by databricks and low-code.
A connector needs to fetch credentials from UC before connecting to an external service to copy data into a Streaming Delta table

a `managed ingestion pipeline` is a type that connects to public SaaS sources to extract data and ingest to a streaming table, they are largely predefined and managed by databricks. For this kind of ingestion all data movement happens in the data plane. The control plane is only for pipeline setup, monitoring, and management.

#### Lakeflow connect
Lakeflow connect is for external databases instead of public API.
It will collect latest state and change logs and store staged data in a UC volume. A serverless job then processes the data from the volume into a streaming delta table so there's an extra step compared to managed connectors.