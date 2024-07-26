# Iceberg Tables Package

The Iceberg Tables Package includes:

* [Snowflake Iceberg table](#snowflake-iceberg-table)
* [External Iceberg table](#external-iceberg-table)
* [Code](#code)

<h2 id="snowflake-iceberg-table">Snowflake Iceberg table</h2>

An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats.[Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

An Iceberg table that uses Snowflake as the Iceberg catalog provides full Snowflake platform support with read and write access. The table data and metadata are stored in external cloud storage, which Snowflake accesses using an external volume. Snowflake handles all life-cycle maintenance, such as compaction, for the table.

### Snowflake Iceberg table Configuration

The Snowflake Iceberg table has two configuration groups:

* [Node Properties](#snowflake-iceberg-table-node-properties)
* [Iceberg Options](#snowflake-iceberg-table-options)

Go to the node and select the **Config tab** to see the Node Properties,Iceberg Options.

<h3 id="snowflake-iceberg-table-node-properties">Snowflake Iceberg table node properties</h3>

There are four configs within the **Node Properties** group.

* **Storage Location(required)**: Storage Location where the Dynamic Table will be created.
* **Node Type(required)**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled(required)**:
  * If TRUE the node will be deployed or redeployed when changes are detected.
  * If FALSE the node will not be deployed or the node will be dropped during redeployment.

<h3 id="snowflake-iceberg-table-options">Iceberg Options</h3>

<h2 id="external-iceberg-table">External Iceberg table</h2>

An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats.[Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

An Iceberg table that uses an external catalog provides limited Snowflake platform support with read-only access. With this table type, Snowflake uses a catalog integration to retrieve information about your Iceberg metadata and schema.

You can use this option to create an Iceberg table registered in the AWS Glue Data Catalog or to create a table from Iceberg metadata files in object storage.

### External Iceberg table Configuration

The Snowflake Iceberg table has two configuration groups:

* [Node Properties](#external-iceberg-table-node-properties)
* [Iceberg Options](#external-iceberg-table-options)

Go to the node and select the **Config tab** to see the Node Properties,Iceberg Options.

<h3 id="external-iceberg-table-node-properties">External Iceberg table node properties</h3>

There are four configs within the **Node Properties** group.

* **Storage Location(required)**: Storage Location where the Dynamic Table will be created.
* **Node Type(required)**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled(required)**:
  * If TRUE the node will be deployed or redeployed when changes are detected.
  * If FALSE the node will not be deployed or the node will be dropped during redeployment.

<h3 id="external-iceberg-table-options">Iceberg Options</h3>

<h1 id="code">Code</h1>


