# PostgreSQL Extension Playground

Learn how to use the new PostgreSQL extension for Visual Studio Code! This repository contains sample databases and instructions for setting up a local PostgreSQL server using Docker, connecting to an Azure PostgreSQL Flexible Server, and using all the features of the new PostgreSQL extension.

* [Prerequisites](#prerequisites)
* [Explore a database schema, tables, and queries](#explore-a-database-schema-tables-and-queries)
* [Use "Chat with this database" feature](#use-chat-with-this-database-feature)
* [GitHub Copilot Agent Mode](#github-copilot-agent-mode)
* [Connect to an Azure PostgreSQL Flexible Server](#connect-to-an-azure-postgresql-flexible-server)

## Prerequisites

* [PostgresSQL](https://www.postgresql.org/download/)
* [PostgreSQL extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-pgsql)
* [Docker Desktop](https://www.docker.com/products/docker-desktop)
* [GitHub Copilot Chat](https://marketplace.visualstudio.com/items/?itemName=GitHub.copilot-chat)

## Explore a database schema, tables, and queries

1. In the PostgreSQL extension, select "Create New Server" and select the local Docker option. Fill in these options:

    | Option | Value |
    |--------|-------|
    | Connection Name | local-postgres |
    | Container Name | local-postgres |
    | Username | postgres |
    | Password | postgres |
    | Save password | checked |
    | Database name | postgres |
    | Server group | Servers (default) |

    Open the "Advanced" section and fill in these options:

    | Option | Value  |
    |--------|--------|
    | Bound Port   | 54876  |

2. Run new query on existing DB (right-click "New Query" on database name):

    ```sql
    CREATE DATABASE adventureworks;
    ```

3. In the terminal, run these commands to restore the data from the schema and data files:

    ```bash
    psql -d "postgres://postgres:postgres@localhost:54876/adventureworks" -f data/adventureworks/adventureworks-schema.sql
    ```

    ```bash
    psql -d "postgres://postgres:postgres@localhost:54876/adventureworks" -f data/adventureworks/adventureworks-data.sql
    ```

4. In the PostgreSQL extension, right-click on the server and select "Refresh". You should see "adventureworks" show up.

5. In the PostgreSQL extension, right-click on the adventureworks database and select "Connect with PSQL". In the psql terminal, try commands like `\dn` to see the schemas in the database, or `\dt sales.*` to list the tables in the `sales` schema.

6. In the PostgreSQL extension, right-click on the adventureworks database and select "Visualize Schema". A window will open with a diagram of the database schema. You can zoom in and out, and click on tables to see their columns and relationships.

7. In the PostgreSQL extension, right-click on the adventureworks database and select "New Query".

8. In the Query editor window, write a new query like:

    ```sql
    SELECT firstname, lastname, city FROM sales.vindividualcustomer
    ```

    The extension should offer to autocomplete the tables from the `sales` schema.

9. When the query is complete, see the results in the "PostgreSQL Query Results" tab below.

10. Select the "Export to CSV" option in the results tab to save the results to a CSV file. You'll also see options to export to XLSX and JSON.

## Use "Chat with this database" feature

1. Reuse the PostgreSQL server you created in the previous section.

2. In the PostgreSQL extension, select the "local-postgres" server you created in the previous section.  Right-click on any database in the server and select "New Query". In the query editor, run the following command to create a new database:

    ```sql
    CREATE DATABASE pagila;
    ```

3. Restore the data from the schema and data files:

    ```bash
    psql -d "postgres://postgres:postgres@localhost:54876/pagila" -f data/pagila/pagila-schema.sql
    ```

    ```bash
    psql -d "postgres://postgres:postgres@localhost:54876/pagila" -f data/pagila/pagila-data.sql
    ```

4. In the PostgreSQL extension, right-click on the server and select "Refresh". You should see "pagila" show up.

5. In the PostgreSQL extension, right-click on the pagila database and select "Chat with this database".

6. In the GitHub Copilot chat window, ask this question:

    @pgsql For each film, find the film that generated the highest total revenue during the first half of 2022. For each, report the total number of rentals, total rental revenue, and percentage of total revenue for each category. Order by highest revenue. Format as a table.

7. In the PostgreSQL extension, open the "Query History" tab, select the query you just ran, and open the query by selecting "Open Query" in the right-click menu.

## Connect to an Azure PostgreSQL Flexible Server

1. Create an Azure PostgreSQL Flexible Server using the Azure Portal, CLI, or Bicep. Currently, you can only use Entra-based authentication if your account is associated with a single tenant. If you are creating a server in a different tenant, you will need to use password-based authentication for now.

2. In the PostgreSQL extension, select "🔌" ("Add New Connection").

3. Select "Browse Azure". Filter the Subscription, Resource Group, and Location to find your server.

4. Configure either the Entra authentication option or password authentication option.

5. Select "Test Connection" to verify that the connection is successful. If no errors pop up, select "Save Connection".

6. In the PostgreSQL extension, you should now see your server listed.

7. In the PostgreSQL extension, right-click on a database in the server and select "Chat with this database".

8. In the GitHub Copilot chat window, ask these questions:

    @pgsql what storage and compute resources does my DB have?

    @pgsql When did the last backup occur?

    @pgsql what extensions are in the allowlist for my server?

    @pgsql Is the DiskANN extension enabled on this server? If not, add it.

## GitHub Copilot Agent Mode

1. In the PostgreSQL extension, select "Create New Server" and select the local Docker option. Fill in these options:

    | Option | Value |
    |--------|-------|
    | Connection Name | local-postgis |
    | Container Name | local-postgis |
    | Username | postgres |
    | Password | postgres |
    | Save password | checked |
    | Database name | postgres |
    | Server group | Servers (default) |

    Open the "Advanced" section and fill in these options:

    | Option | Value |
    |--------|--------|
    | Bound Port   | 56453  |
    | Image name | postgis/postgis |
    | Image version | 16 |

    If you are on an ARM machine, you may need to use ["imresamu/postgis"](https://hub.docker.com/r/imresamu/postgis) as the image instead.

    | Option | Value |
    |--------|--------|
    | Image name | imresamu/postgis |
    | Image version | 16-3.4-bundle0-bookworm |

2. Open the GitHub Copilot Chat panel, and select the "Agent" mode.

3. In the Agent mode, you can ask questions about your database and get answers in natural language.

    ```text
    Create a new database in "local-postgis" server called "observations" and enable postgis for it
    ```

    ```text
    Use the data/observations/pennsylvania-insects.csv from my workspace to create a new table called "pennsylvania" in the "local-postgis" server and load the data in.
    ```

    ```text
    Visualize the schema
    ```

    ```text
    Convert the latitude longitude into a new geometry column
    ```

    ```text
    what are the top observed species in philadelphia PA?
    ```

## Licensing

This repository is provided under the MIT License. However, please note that each individual database included in this repository is subject to its own license terms.

The MIT License applies to the scripts and other components that we created. We respect the rights of the original creators of the databases, and we only redistribute these databases in compliance with their respective licenses.

For each individual database, we have clearly indicated where the full text of the license can be found. If you choose to use any of these databases, you must comply with the terms specified in their respective licenses.
