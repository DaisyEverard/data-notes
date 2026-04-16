What is it? An open-source protocol for reading and writing files to cloud storage. 

Data is stored as parquet files in a directory. This will have a sub-directory called `_delta_log` with transaction logs and metadata in json files.

key features: Full [[ACID transactions]], time travel, schema enforcement while allowing schema evolution, batch and streaming support