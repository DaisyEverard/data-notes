Idempotency - Tracking files which have previously been ingested and only taking the delta

[[RDDs]] - Resilient Distributed Datasets
RDPGs - resilient distributed property graphs

**Commuative Process** - a process where order does not matter
**Associative Process** - a process where grouping does not matter.
simple addition is associative and commutative  `a+b+c=c+b+a= c+(b+a)`
multiplication is commutative and associative `abc = (cb)a = c(ba) `
subtraction is not associative or commutative` a-b-c != (c-b)-a != c-(b-a)`
division is not associative or commutative `a/b/c != (c/b)/a != c/(b/a)`
string concatenation is associative but not commutative. `f"{ab}{c}" = f"{a}{b}{c}" != f"{b}{c}{a}"` ^cd7ae8