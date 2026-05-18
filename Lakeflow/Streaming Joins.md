### Streaming + Static
aka a Stream-Snapshot join or a Stream-Static join.
Incrementally join new data from streaming to static. Only joins the streaming delta each time so is relatively efficient.
The output is a new streaming table.

example: enriching streaming data by joining area code to area lookup for full string

### Streaming + Streaming (Materialized View)
joins all rows from both streaming tables each time the pipeline is run.
If both sides are streaming you require a materialised view to keep results current and joins efficient. new rows from both tables are incrementally refreshed depending on pipeline config and compute mode.

### Streaming + Streaming (Incremental)
Processed as data arrived. Only new data from each stream is joined not considering past data.
Usually involves windowing logic, watermarking, and other advanced streaming concepts

example: clickstream data, real time analytics