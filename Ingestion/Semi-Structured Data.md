Data is an example of semi-structured data where you may have to deal with deeply nested data.

This wil initially be ingested as a long string into bronze. You can access the values in multiple ways

##### As a string:
using colons e.g
`json_column:key1:key2` will get the value for key 2 nested 2 layers deep. This is not recommended.

##### As a struct:
Parse JSON into a struct by defining a schema which will be enforced unlike raw strings. This is more efficient to query than strings.
json Object = struct STRUCT<>
json Array = struct ARRAY<>

for instance the type of a simple json column converted to struct could be
```
STRUCT<
	name: STRING,
	age: INT,
	children: ARRAY<
	STRUCT<
	  name: STRING
	  >
    >
 >
```

You can use the function `schema_of_json(<json_string>)` to let databricks parse this instead of defining it manually

Convert the type from string to struct by doing
```
SELECT
from_json(string_column, 'struct_schema_as_string') AS struct_column
FROM table
```

##### As a Variant
the VARIANT dtype was introduced in 2025. It is designed for semi-structured data and has greater performance than other methods