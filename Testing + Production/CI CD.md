Continuous integration is when developers regularly commit code, build, test and release code to a shared repository. The goal is to catch issues early through continuous integration

Continuous deployment automates the release of code to production after passing automated tests. The goal is to deliver features and fixes quickly to prod while avoiding errors.

There are 2 ways you can separate environments like dev, staging, and prod in Databricks. You can either use multiple workspaces, one for each env, or a single workspace with multiple catalogs. Generally complex solutions require multiple catalogs in the same environment so the separate workspaces solution is usually more flexible.

### Deployment
There are 3 main methods

REST API - postman and Databricks
Databricks CLI - wraps rest API for one off tasks, experimentation, and shell scripting
SDKs - available for python, java, go, and R. Programmatic way

Ease of use: SDK > CLI > REST API
Flexibility: REST API > SDK > CLI


#### DABs (Declarative Automation Bundles)
YAML files that specify artifacts, resources, and configurations of a Databricks project to make it easier to configure and reproduce complex pipelines.
They are easily stored by git and treat Databricks resources as code.