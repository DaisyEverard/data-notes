## Traditional Batch
all records processed each run

CREATE TABLE AS SELECT
spark.read.load()

## Incremental Batch
Like batch but only new records ingested.

COPY INTO
spark.readStream(autoloader with scheduled trigger)
Declarative Pipelines (CREATE OR REFRESH STREAMING TABLE)

## Streaming
near real-time
micro-batches
good for kafka, Google pub/sub, apache pulsar

spark.readStream(autoloader with continous trigger)
Delarative Pipelines (trigger mode continuous)