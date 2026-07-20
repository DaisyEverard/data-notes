Serialization is converting an object into a form that can be persisted or transported. The opposite, deserialization, is converting a stream into an object.

When working with custom UDFs the function must be serialized and sent to every executor in a cluster. You must also pass parameters and return values for each invocation of the UDF for every row of distributed data. This means computation can balloon.
Python UDFs are even more expensive than SQL ones as they need to be pickled and each executor needs to instantiate a python interpreter. The catalyst optimizer can't connect code before and after UDFs so created a black box and limits optimizations.
Avoid UDFs where at all possible. If you have to use python UDFs use Vectorized UDFs.