Both are modelling approaches to meet compliance
![[Data Protection Techniques Overview.jpg]]
### Pseudonymization
 Changing a piece of plain information to an identifier which is equally unique, for instance John Smith -> User-123. Only authorized users will have access to the data linking a key to a real name.
- protects at record level
- re-identification is possible later
- Still considered PII
- Easy to delete and know other data hasn't been compromised

Hashing means applying a function such as SHA to PII to get a random string of characters. hashes are deterministic so the same string in always equals the same string out. You can 'salt' hashes by adding random strings to values before hashing.
Increases data size and makes some operations less efficient.

Tokenization converts PII to keys which are stored with real values in a secure lookup table. Slow to write but fast to read. The de-identification data can be stored in fewer bytes.
Keys are usually encoded in a long value.

### Anonymization
Hiding or changing information so it isn't any kind of key. For instance John Smith -> `*****`
- protects entire dataset mostly for business intelligence
- irreversibly altered so subject can't be directly or indirectly identified
- not linkable to original data
- multiple anonymization methods usually used in real-world scenarios

Data Suppression excludes columns with PII from view, removes rows where demographic groups are too small, and uses dynamic access controls for conditional access to full data.

Generalization removes specificity. Different data type needs different generalization methods such as:
**categorical generalization** - e.g. stating a country instead of a region or postcode, or using a general business unit instead of office for a survey.
**Binning** - e.g. a set of 10 year age ranges instead of a specific age. age = 20-30. or Salary bands. Meaningful divisions require good domain expertise. Splitting continuous data into 'bins'.
**Truncating IP addresses** - rounding IP address to /24 CIDR, replacing last byte with 0. Generalizes IP geolocation to city or neighbourhood level. 10.5.56.45 -> 10.5.56.0/24
**Rounding** - round to the neared reasonable level. e.g. nearest $1000 for salary. Assumes data is normally distributed so data is equally rounded up and down and doesn't change trends when aggregated.