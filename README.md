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

* **Snowflake EXTERNAL VOLUME name**: Specifies the identifier (name) for the external volume where the Iceberg table stores its metadata files and data in Parquet format.[External volume](https://docs.snowflake.com/sql-reference/sql/create-external-volume) needs to be created in snowflake as a prerequisite.
* **Base location name**: The path to a directory where Snowflake can write data and metadata files for the table. Specify a relative path from the table’s EXTERNAL_VOLUME location. 
* **Cluster key**: True/False to determine whether the Iceberg table is to be clustered or not
  * True -Allows you to specify the column based on which clustering is to be done.
            -Allow Expressions Cluster Key-True ->allows to add an expression to the specified cluster key
  * False – No clustering done

 ## Prerequisites
 
 * The Role we mention in the Workspace and Environment properties of Coalesce should be 'ACCOUNTADMIN' inorder to successfully create an  iceberg table
 * An EXTERNAL VOLUME is expected to be created in Snowflake at the Storage Location chosen in the Node properties
 * In case of creating a snowflake iceberg table with structured column type like OBJECT,MAP or ARRAY.Ensure the data type is updated with the appropriate structure.For example,the source snowflake table has OBJECT data type,then the datat type of the same column in the iceberg table node added on top is expected to structured type OBJECT(age string,id number) based on the data it has.

### Snowflake Iceberg table Initial Deployment

When deployed for the first time into an environment the Dynamic Table Latest Record Version node will execute the below stage:

* **Create Iceberg Table**: This stage will execute a `CREATE OR REPLACE` statement and create a Dynamic Table in the target environment.

### Snowflake Iceberg table Redeployment


#### Recreating a Snowflake Iceberg table

If any changes in config options like external volume,baselocation ,node properties ,column  results in recreating iceberg table during redeployment

* **Create Iceberg Table**: This stage will execute a `CREATE OR REPLACE` statement and create a Dynamic Table in the target environment.

###  Alter structured data type

Changes in structured data type columns excluding any other config changes results in alter statements execution.

* **Alter structured data type in Iceberg Table**:

###  Snowflake Iceberg table Undeployment

If a snowflake iceberg table is dropped from the workspace and commited to GIT results in table dropped from target environment.

This is executed as a single stage:

* **Drop Iceberg Table**
  
<h2 id="external-iceberg-table">External Iceberg table</h2>

External Iceberg table node is an external iceberg table node wrapped with a task functionality.

An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats.[Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

An Iceberg table that uses an external catalog provides limited Snowflake platform support with read-only access. With this table type, Snowflake uses a catalog integration to retrieve information about your Iceberg metadata and schema.

You can use this option to create an Iceberg table registered in the AWS Glue Data Catalog or to create a table from Iceberg metadata files in object storage.

### External Iceberg table Configuration

The Snowflake Iceberg table has three configuration groups:

* [Node Properties](#iceberg-table-node-properties)
* [Iceberg Options](#iceberg-table-options)
* [Scheduling Options](#iceberg-task-scheduling-options)

Go to the node and select the **Config tab** to see the Node Properties,Iceberg Options and Scheduling Options.

<h3 id="iceberg-table-node-properties">External Iceberg table node properties</h3>

There are four configs within the **Node Properties** group.

* **Storage Location(required)**: Storage Location where the Dynamic Table will be created.
* **Node Type(required)**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled(required)**:
  * If TRUE the node will be deployed or redeployed when changes are detected.
  * If FALSE the node will not be deployed or the node will be dropped during redeployment.

<h3 id="iceberg-table-options">Iceberg Options</h3>

* **Type of Catalog**:Specify the type of catalog
                    *AWS Glue
                    *Object Storage
* **Snowflake EXTERNAL VOLUME name**: Specifies the identifier (name) for the external volume where the Iceberg table stores its metadata files and data in Parquet format.[External volume](https://docs.snowflake.com/sql-reference/sql/create-external-volume) needs to be created in snowflake as a prerequisite.
* **Catalog integration**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.
* **Catalog namespace**:Optionally specifies the namespace (for example, my_glue_database) for the AWS Glue Data Catalog source.Option available if AWS Glue catalog is chosen.
* **Catalog table name**:Name of the catalog table.Option available if AWS Glue catalog is chosen.
* **Metadata filepath**:Specifies the relative path of the Iceberg metadata file to use for column definitions.Option available if Object Storage Catalog is chosen.
* **Schedule refresh**: True / False toggle that determines whether a task will be created or if the SQL to be used in the task will execute as DML as a Run action. Prior to creating a task, it is helpful to test the SQL the task will execute to make sure it runs without errors and returns the expected data.
    * False - A table will be created and SQL will execute as a Run action.
    * True - After sufficiently testing the SQL as a Run action, setting Schedule refresh Mode to true will wrap the SQL statement in a task with options specified in Scheduling Options.


<h3 id="iceberg-task-scheduling-options">Scheduling Options</h3>

If schedule refresh mode is set to true then Scheduling Options can be used to configure how and when the task will run.

* **Scheduling Mode**: Specifies whether a warehouse or serverless compute is used to run the task 
    * Warehouse Task - User managed warehouse will execute tasks. 
    * Serverless Task - Utilize serverless compute to execute tasks.
* **Select Warehouse on which to run task**: Visible if Scheduling Mode is set to Warehouse Task
    * Enter the name of the warehouse you want the task to run on without quotes
* **Select initial serverless Warehouse size**: Visible when Scheduling Mode is set to Serverless Task
    * Select the initial compute size on which to run the task. Snowflake will adjust size from there based on target schedule and task run times.
* **Task Schedule**: Select how you want to schedule the task to run
    * **Minutes** - Allows you to specify a minute interval for running task
    * **Cron** - Allows you to specify a CRON schedule for running task
    * **Predecessor** - Allows you to specify a predecessor task to determine when a task should execute
* **Enter task schedule using minutes**: Only visible when Task Schedule is set to Minutes. Enter a whole number from 1 to 11520 which represents the number of minutes between task runs.
* **Enter task schedule using Cron**: Only visible when Task Schedule is set to Cron. Specifies a cron expression and time zone for periodically running the task. Supports a subset of standard cron utility syntax.
* **Enter predecessor task(s) separated by a comma**: Only visible when Task Schedule is set to Predecessor. One or more task names that precede the task being created in the current node. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor tasks separate the task names using a comma and no spaces.
* **Enter root task name**: Name of the root task that controls scheduling for the DAG of tasks. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor tasks separate the task names using a comma and no spaces.

 ## Prerequisites
 
 * The Role we mention in the Workspace and Environment properties of Coalesce should be 'ACCOUNTADMIN' inorder to successfully create an  iceberg table.You can also grant SYSADMIN roles to EXTERNAL VOLUME,CATALOG INTEGRATION created.
 * An EXTERNAL VOLUME,CATALOG INTEGRATION is expected to be created in Snowflake at the Storage Location chosen in the Node properties.
   
### External Iceberg table - System Columns

* **DATA** - Column added for deployment but column is not added to iceberg table as columns specifications are not required for External Iceberg tables.
  
#### Iceberg tables  With Task Deployment Parameters

The Iceberg tables  With Task includes an environment parameter that allows you to specify a different warehouse used to run a task in different environments.

The parameter name is `targetTaskWarehouse` and the default value is `DEV ENVIRONMENT`.

When set to `DEV ENVIRONMENT` the value entered in the Scheduling Options config Select Warehouse on which to run the task will be used when creating the task.

```json
{
    "targetTaskWarehouse": "DEV ENVIRONMENT"
}
```

When set to any value other than `DEV ENVIRONMENT` the node will attempt to create the task using a Snowflake warehouse with the specified value.

For example, with the below setting for the parameter in a QA environment, the task will execute using a warehouse named `compute_wh`.

```json
{
    "targetTaskWarehouse": "compute_wh"
}
```

 ### External Iceberg table Initial Deployment

When deployed for the first time into an environment the Work with Task node will execute three stages dependent on whether or not the task schedule relies on a predecessor task:

#### External Iceberg table No Predecessor Task Deployment

* **Create Iceberg Table**: This stage will execute a `CREATE OR REPLACE` statement and create a Iceberg Table in the target environment.
* **Create Task**: This stage will create a task that will load the target table on the schedule specified
* **Resume Task**: After the task has been created it needs to be resume so that the task runs on the schedule
 
#### External Iceberg table Predecessor Task Deployment

* **Create Iceberg Table**: This stage will execute a `CREATE OR REPLACE` statement and create a Iceberg Table in the target environment.
* **Suspend Root Task**: To add a task into a DAG of task the root task needs to be put into a suspended state
* **Create Task**: This stage will create a task that will load the target table on the schedule specified

If a task is part of a DAG of tasks, the DAG needs to include a node type called **Task Dag Resume Root**. This node will resume the root node once all the dependent tasks have been created as part of a deployment.

The task node has no ALTER capabilities. All task enabled nodes are CREATE OR REPLACE only though this is subject to change.

### External Iceberg table Redeployment

#### Recreating a Snowflake Iceberg table

If any changes in config options like external volume,baselocation ,node properties ,column  results in recreating iceberg table during redeployment

* **Create Iceberg Table**: This stage will execute a `CREATE OR REPLACE` statement and create a Iceberg Table in the target environment.
  
#### External Iceberg table Recreating the Task Redeployment

After the Task has deployed for the first time into a target environment, subsequent deployments with changes in task schedule, warehouse, or scheduling options will result in a CREATE TASK AND RESUME TASK statements being issued

The following stages are executed:

####  External Iceberg table No Predecessor Task Redeployment

* **Create Task**: This stage will create a task that will load the target table on the schedule specified.
* **Resume Task**: After the task has been created it needs to be resume so that the task runs on the schedule.
	 
#### External Iceberg table Predecessor Task Redeployment

* **Suspend Root Task**: To add a task into a DAG of task the root task needs to be put into a suspended state.
* **Create Task**: This stage will create a task that will load the target table on the schedule specified


###  External Iceberg table Undeployment

If a snowflake iceberg table with task  is dropped from the workspace and commited to GIT results in table and task dropped from target environment.
This is executed in the below stages:

* **Drop Iceberg Table**

#### External Iceberg table No Predecessor Task Undeployment

* **Drop Iceberg Table**
* **Drop Current Task**: This stage will drop the task.

#### External Iceberg table Predecessor Task Undeployment

* **Drop Iceberg Table**
* **Suspend Root Task**: To drop a task from a DAG of task the root task needs to be put into a suspended state.
* **Drop Task**: This stage will drop the task.

If a task is part of a DAG of tasks the DAG needs to include a node type called **Task Dag Resume Root**. This node will resume the root node once all the dependent tasks have been created as part of a deployment.

<h1 id="code">Code</h1>

## Snowflake Iceberg table

* [Node definition](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/SnowflakeIcebergtable-305/definition.yml)
* [Create Template](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/SnowflakeIcebergtable-305/create.sql.j2)

## External Iceberg table

* [Node definition](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/ExternalIcebergtable-307/definition.yml)
* [Create Template](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/ExternalIcebergtable-307/create.sql.j2)

