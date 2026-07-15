There are built in methods in pyspark.testing.utils to help tests. For instance:
-  assertDataFrameEqual(actual, expected)
- assertSchemaEqual(actual, expected)

### Pytest
looks for functions in a file starting with `test_` automatically.
Framework for unit tests