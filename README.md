# Iceberg Tables Package

The Iceberg Tables Package includes:

* [Snowflake Iceberg table](#snowflake-iceberg-table)
* [External Iceberg table](#external-iceberg-table)
* [Code](#code)

<h2 id="snowflake-iceberg-table">Snowflake Iceberg table</h2>

An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats.[Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

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

### Snowflake Iceberg table Configuration

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


