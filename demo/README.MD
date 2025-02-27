# Demo

The contents of this Demo serves two audiences - Consumers and Contributors. For the Consumer, the demo provides a basis
to see how schemachange works with the main feature set. For the contributor, who forks the repo and submits PRs to the
codebase, this will serve as a basis to test the PR against your own snowflake account to ensure your code change does
not break any existing functionality.

## Prerequisites

- You will need your own snowflake Account to test the Demo - Both as a contributor and consumer.
- You will need to review and run statements in the provision folder or set up your own database and schema.
    - [initialize.sql](provision/initialize.sql): Contains the SQL variables to initialize your environment.
    - [setup_schemachange_schema.sql](provision/setup_schemachange_schema.sql): Contains the SQL variables to track the
      individual demo scenarios in its own change history table.

### Consumers

- If you are consumer who is installing schemachange and wants to test-run the demo, then you will have to set the
  following environment variables.
    - SNOWFLAKE_ACCOUNT: This will be the account identifier for your Snowflake account.
    - SNOWFLAKE_USER: This will be the user that will connect to your Snowflake account (`SNOWFLAKE_ACCOUNT`).
    - SNOWFLAKE_PASSWORD: This is the password for the user (`SNOWFLAKE_USER`) that will connect to your Snowflake
      account (`SNOWFLAKE_ACCOUNT`).
    - SCENARIO_NAME: This will be demo folder you intend to experiment with. For
      starters, `basics_demo`, `citibike_demo` or `citibike_demo_jinja` are included with the repo that will set the
      root folder value in the respective schemachange-config.yml file.
    - SNOWFLAKE_WAREHOUSE: This will be the warehouse you set up for the demo in your Snowflake
      account (`SNOWFLAKE_ACCOUNT`). Default setup is SCHEMACHANGE_DEMO_WH.
    - SNOWFLAKE_DATABASE Keyed to SCHEMACHANGE_DEMO. You can update this to the database in your snowflake account
      (`SNOWFLAKE_ACCOUNT`). You will also need to update the provision scripts accordingly.
    - SNOWFLAKE_ROLE Keyed to SCHEMACHANGE_DEMO-DEPLOY. This particular role name was chosen to test hyphenated roles
      in schemachange. You can update this value to match the role you have setup in your snowflake
      account (`SNOWFLAKE_ACCOUNT`).

The scripts in the `provision` folder can be used to set up up your demo database along with a schema in that database
that will house the change tracking tables needed to set up and teardown the schemas used to test a working version of
the demo DDL scripts.

- The [initialize](provision/initialize.sql) script setups the database, warehouse and account level access roles that
  will be used on the
- The [setup](provision/setup_schemachange_schema.sql) script creates the `SCHEMACHANGE` schema in the database that you
  created in the initialize step.

### Contributors

1. Execute the [initialize.sql](provision/initialize.sql)
   and [setup_schemachange_schema.sql](provision/setup_schemachange_schema.sql) scripts to create up a
   `SCHEMACHANGE_DEMO`
   database and `SCHEMACHANGE` schema (See [Prerequisites](#prerequisites)).

2. Create the
   following [GitHub Action Secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)
   in your forked repository. GitHub will use these actions to set up and teardown the temporary schema(s) it creates to
   test your code.

    - `SCHEMACHANGE_SNOWFLAKE_PASSWORD`
    - `SCHEMACHANGE_SNOWFLAKE_USER`
    - `SCHEMACHANGE_SNOWFLAKE_ACCOUNT`

# Setup

The setup scripts are included to build the schema needed by the GitHub Actions Workflow to avoid conflict across jobs
when tested in parallel. The Setup script will create a new schema to run the schemachange script for the corresponding
scenario.

# Teardown

The Teardown scripts are the bookend pairing of the Setup script for each scenario so that when the build process is
done using GitHub actions, you will have a log of the testing done to ensure that schemachange is working as expected.
