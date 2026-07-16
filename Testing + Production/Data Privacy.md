Regulations in the EU = GDPR (General Data Protection Regulation), in the US = CCPA (California Consumer Privacy Act)

For a successful strategy, for each piece of data, you need to know:
- Where data is sourced, stored, and consumed
- What kind of data it is. e.g. PII, address, public
- What security has been applied
- If data is or should be anonymized or pseudonymized
- How to recover secured data
- How to search data and process the right to be forgotten, rectified, or restrict processing.


Traditional Data lakes have no fine-grained access control and need a different governance model for every cloud, model, and database. Unity catalog means one management system across all assets with row and column level granularity access controls and tagging. There is also auto-capture of runtime data lineage across tables, columns, dashboards, Jobs, notebooks, files, external sources, and models.

ACLs (access control lists) control who can access what object, and which privileges they have on it such as USE CATALOG, SELECT, CREATE TABLE etc.Data access can be managed via SQL, the catalog explorer, or programmatically with the CLI; REST APIs or Terraform.

When you don't have access to a specific row or column it is simply omitted from the result rather than showing up as 'hidden' or something similar.
Masking means partially obscuring or transforming values rather than completely redacting them. For instance part of an email or account number being replaced with \* such as `*****gmail.com`
Row-Level Security and Column masking - fine grained access control for streaming and materialized tables

This is an example of a row filter.
the if function has the syntax `IF(condition, value_if_true, value_if_false)`
It is TRUE for users in the admin group
it is TRUE for non-admin users when region is US
it is FALSE for non-admin users for any other region
```
CREATE FUNCTION
us_filter(region STRING)
RETURN IF(IS_MEMBER('admin'), true, region="US");

ALTER TABLE sales SET ROW FILTER us_filter ON region;
```

This is a generic masking function
```
CREATE FUNCTION mask_if_not_admin(column STRING)
RETURN CASE WHEN IS MEMBER('admin') THEN column ELSE '*****' END;

ALTER TABLE users ALTER COLUMN column SET MASK mask_if_not_admin
```
For row filtering and column masks you need to use UDFs to attach them to the table so it may reduce the efficiency of your processing and not be the most efficient approach.

- Build-in data skipping optimizations (z-order) and housekeeping of obsolete/deleted data (VACUUM)
- Uses transactional logs for auditing