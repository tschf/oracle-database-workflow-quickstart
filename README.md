# Oracle Database Workflow Quickstart [![database tests](https://github.com/tschf/oracle-database-workflow-quickstart/actions/workflows/db_tests.yml/badge.svg)](https://github.com/tschf/oracle-database-workflow-quickstart/actions/workflows/db_tests.yml)

This repository is a reference to creating a GitHub Workflow to test your code
against an Oracle Database. This repo leverages the 23ai Free version - that Gerald
Venzl has kindly published to the Marketplace for easy uptake.

## Workflow Steps

| Step Name                | Description                                                      | Reference Link |
| ---                      | ---                                                              | ---|
| setup_db_credentials     | Randomly assign password values for sys and application user     | |
| checkout files           | Checkout the files from the active branch | https://github.com/marketplace/actions/checkout |
| create podman network    | Create a podman network (db-net) so the sqlcl can reference the DB container by name | |
| db install               | Set up the database - which is 23ai Free                         | <https://github.com/gvenzl/setup-oracle-free> |
| db install logs          | Output the logs from the db install                              | |
| download sqlcl image     | Download the sqlcl image from container registry. There is an example from Geral's repo, however I found pulling the container image to be much quicker.  | |
| (dba) prepare db         | Perform any DBA tasks. Here I call the script `setup_scripts/_setup_scripts.sql` which creates a `DATA` tablespace and a new user. There is a setting `setup-scripts` where the DB container can run scripts on set up but I was getting inconsistent behaviour so I thought it will be better just to handle this ourselves after DB setup step. |
| APEX install             | 23ai Free image doesn't come pre-loaded with APEX. So here I install APEX runtime to get the core objects. This is the longest running process at about 3.5 minutes. | |
| install project          | Finally set up is complete - this is where you would kick off your application install to see everything installs as expected| |

## Implementation Steps

The following are the steps to implement this in your project:

1. Copy the file `.github/workflows/db_tests.yml` in your project
2. If your project has no APEX dependency, remove the step APEX install
3. Update the step `(dba) prepare db` to call a script for any DBA setup required - such as user creation
4. Update the step `install project` to call your installation scripts

## Other Useful Documentation

| Doc Name                       | Link |
| ---                            | ---  |
| Workflow Synax                 | <https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions> |
| Context Variables              | <https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#github-context> |
| SQLcl Container Image          | <https://container-registry.oracle.com/ords/ocr/ba/database/sqlcl> |
| GitHub Actions Billing Details | <https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions#included-storage-and-minutes> |
