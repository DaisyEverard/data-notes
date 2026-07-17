Metastores provide regional isolation but are not considered part of the data hierarchy.
Catalogs are the primary unit of data isolation.

There are 2 options for governance model.
**Centralized** means admins are owners of the **metastore** can can take ownership of any object or permissions
**Distributed** means the **catalog** or set of catalogs is the data domain. The owner can create and own all assets. Owners can operate independently of owners of other domains.

You can configure storage locations at the metastore, catalog, or schema level to satisfy the need to have certain data types stored in separate accounts or buckets.
**External Locations** are a path to cloud storage + a storage credential. UC can read and write data on your cloud tenant. You can register external tables and external volumes in UC with external loations. These can be bound to specific workspaces.

These work by making a query through a cluster. The request is sent to UC which logs it then validates the query against security constraints defined in the metastore. UC assumes the credential of the SP and generates a scoped temporary token for data access and an access URL.
![[external access process.png]]

You can isolate with workspaces, you can also isolate with cluster type.
single-use clusters are for one user
multi-user clusters support users with different privileges.

Data in transit, such as between the control plane and users or compute plane, is encrypted using TLS/SSL protocols
Data at rest in Azure storage or AWS EBS volumes uses AES-256 encryption.
Control plane data uses envelope encryption where a DEK (data encryption key) is encrypted with a CMK (customer managed key) and then re-encrypted with a Databricks managed key