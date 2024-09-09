# Iceberg Tables Package

The Iceberg Tables Package includes:

* [Snowflake Iceberg table](#snowflake-iceberg-table)
* [External Iceberg table](#external-iceberg-table)
* [Copy-Into Iceberg table](#copy-into-iceberg-table)
* [Snowpipe Iceberg table](#snowpipe-iceberg-table)
* [Code](#code)

* [Code](#code)

<h2 id="snowflake-iceberg-table">Snowflake Iceberg table</h2>

An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats. [Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you can't, or choose not to, store in Snowflake.

An Iceberg table that uses Snowflake as the Iceberg catalog provides full Snowflake platform support with read and write access. The table data and metadata are stored in external cloud storage, which Snowflake accesses using an external volume. Snowflake handles all life-cycle maintenance, such as compaction, for the table.

### Snowflake Iceberg Table Prerequisites

* The Role we mention in the Workspace and Environment properties of Coalesce should be `ACCOUNTADMIN` in order to successfully create a  Iceberg table.
* An EXTERNAL VOLUME is expected to be created in Snowflake at the Storage Location chosen in the Node properties.
* In case of creating a Snowflake Isceberg table with structured column type like `OBJECT`, `MAP` or `ARRAY`. Ensure the data type is updated with the appropriate structure. For example,the source Snowflake table has OBJECT data type,then the data type of the same column in the Iceberg table node added on top is expected to structured type OBJECT (age string,id number) based on the data it has.

### Snowflake Iceberg Table Configuration

The Snowflake Iceberg table has two configuration groups:

* [Node Properties](#snowflake-iceberg-table-node-properties)
* [Iceberg Options](#snowflake-iceberg-table-options)

<h3 id="snowflake-iceberg-table-node-properties">Snowflake Iceberg Table Node Properties</h3>

There are four configs within the **Node Properties** group.

* **Storage Location(required)**: Storage Location where the Dynamic Table will be created.
* **Node Type(required)**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled(required)**:
  * If TRUE the node will be deployed or redeployed when changes are detected.
  * If FALSE the node will not be deployed or the node will be dropped during redeployment.

<h3 id="snowflake-iceberg-table-options">Iceberg Options</h3>

* **Type of Catalog**:
     * Snowflake
     * Polaris
* **Snowflake EXTERNAL VOLUME name**: Specifies the identifier (name) for the external volume where the Iceberg table stores its metadata files and data in Parquet format. [External volume](https://docs.snowflake.com/sql-reference/sql/create-external-volume) needs to be created in snowflake as a prerequisite.
* **Base location name**: The path to a directory where Snowflake can write data and metadata files for the table. Specify a relative path from the table’s EXTERNAL_VOLUME location.
* **Catalog integration**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.This option is enbled if the type of catalog is Polaris
* **Cluster key**: True/False to determine whether the Iceberg table is to be clustered or not. Setting it to True for the following options:
  * True: Allows you to specify the column based on which clustering is to be done.
    * Allow Expressions Cluster Key: True Allows to add an expression to the specified cluster key
    * False: No clustering done

### Snowflake Iceberg Table Initial Deployment

When deployed for the first time into an environment the Dynamic Table Latest Record Version node will execute the below stage:

* **Create Iceberg Table**: This stage will execute a `CREATE` OR `REPLACE` statement and create a Dynamic Table in the target environment.

### Snowflake Iceberg Table Redeployment

If any changes in config options like `EXTERNAL VOLUME`, `Base location name`, `Node Properties`, `Column Name` results in recreating Iceberg table during redeployment.

#### Recreating a Snowflake Iceberg Table

* **Create Iceberg Table**: This stage will execute a `CREATE` OR `REPLACE` statement and create a Dynamic Table in the target environment.

### Alter Structured Data Type

Changes in structured data type columns excluding any other config changes results in `ALTER` statements execution.

#### Alter Structured Data Type in Iceberg Table
  
This stage will execute `ALTER ICEBERG TABLE` statement and alters structured data types like OBJECT,MAP or ARRAY

### Snowflake Iceberg Table Dropped

If a Snowflake Iceberg table is dropped from the workspace and commited to Git results in table dropped from target environment.

This is executed as a single stage:

* **Drop Iceberg Table**
  
<h2 id="external-iceberg-table">External Iceberg Table</h2>

External Iceberg table node is an external Iceberg table node wrapped with task functionality.

An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats. [Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.

An Iceberg table that uses an external catalog provides limited Snowflake platform support with read-only access. With this table type, Snowflake uses a catalog integration to retrieve information about your Iceberg metadata and schema.

You can use this option to create an Iceberg table registered in the AWS Glue Data Catalog or to create a table from Iceberg metadata files in object storage.

### External Iceberg Table Prerequisites

* The Role in the Workspace and Environment properties of Coalesce should be `ACCOUNTADMIN` inorder to successfully create an  Iceberg table. You can also grant `SYSADMIN` roles to `EXTERNAL VOLUME`, `CATALOG INTEGRATION`created.
* An `EXTERNAL VOLUME`, `CATALOG INTEGRATION` is expected to be created in Snowflake at the Storage Location chosen in the Node properties.

### External Iceberg Table Configuration

The Snowflake Iceberg table has three configuration groups:

* [External Iceberg Table Node Properties](#iceberg-table-node-properties)
* [External Iceberg Table Options](#iceberg-table-options)
* [External Iceberg Table Scheduling Options](#iceberg-task-scheduling-options)

<h3 id="iceberg-table-node-properties">External Iceberg Table Node Properties</h3>

There are four configs within the **Node Properties** group.

* **Storage Location(required)**: Storage Location where the Dynamic Table will be created.
* **Node Type(required)**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled(required)**:
  * If TRUE the node will be deployed or redeployed when changes are detected.
  * If FALSE the node will not be deployed or the node will be dropped during redeployment.

<h3 id="iceberg-table-options">External Iceberg Table Options</h3>

* **Type of Catalog**: Specify the type of catalog.
  * AWS Glue
  * Object Storage
* **Snowflake EXTERNAL VOLUME name**: Specifies the identifier (name) for the external volume where the Iceberg table stores its metadata files and data in Parquet format. [External volume](https://docs.snowflake.com/sql-reference/sql/create-external-volume) needs to be created in Snowflake as a prerequisite.
* **Catalog integration**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.
* **Catalog namespace**: Optionally specifies the namespace (for example, `my_glue_database`) for the AWS Glue Data Catalog source. Option available if AWS Glue catalog is chosen.
* **Catalog table name**: Name of the catalog table. Option available if AWS Glue catalog is chosen.
* **Metadata filepath**: Specifies the relative path of the Iceberg metadata file to use for column definitions. Option available if Object Storage Catalog is chosen.
* **Schedule refresh**: True or False toggle that determines whether a task will be created or if the SQL to be used in the task will execute DML as a Run action. Prior to creating a task, it is helpful to test the SQL the task will execute to make sure it runs without errors and returns the expected data.
  * False - A table will be created and SQL will execute as a Run action.
  * True - After sufficiently testing the SQL as a Run action, setting Schedule refresh Mode to true will wrap the SQL statement in a task with options specified in Scheduling Options.

<h3 id="iceberg-task-scheduling-options">External Iceberg Table Task Scheduling Options</h3>

If schedule refresh mode is set to true then Task Scheduling Options can be used to configure how and when the task will run.

* **Scheduling Mode**: Specifies whether a warehouse or serverless compute is used to run the task.
  * Warehouse Task - User managed warehouse will execute tasks.
  * Serverless Task - Utilize serverless compute to execute tasks.
* **Select Warehouse on which to run task**: Visible if Scheduling Mode is set to Warehouse Task.
  * Enter the name of the warehouse you want the task to run on without quotes
* **Select initial serverless Warehouse size**: Visible when Scheduling Mode is set to Serverless Task.
  * Select the initial compute size on which to run the task. Snowflake will adjust size from there based on target schedule and task run times.
* **Task Schedule**: Select how you want to schedule the task to run.
  * **Minutes** - Allows you to specify a minute interval for running task.
  * **Cron** - Allows you to specify a CRON schedule for running task.
  * **Predecessor** - Allows you to specify a predecessor task to determine when a task should execute.
* **Enter task schedule using minutes**: Only visible when Task Schedule is set to Minutes. Enter a whole number from 1 to 11520 which represents the number of minutes between task runs.
* **Enter task schedule using Cron**: Only visible when Task Schedule is set to Cron. Specifies a cron expression and time zone for periodically running the task. Supports a subset of standard cron utility syntax.
* **Enter predecessor tasks separated by a comma**: Only visible when Task Schedule is set to Predecessor. One or more task names that precede the task being created in the current node. Task names are case sensitive and should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor tasks separate the task names using a comma and no spaces.
* **Enter root task name**: Name of the root task that controls scheduling for the DAG of tasks. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor tasks separate the task names using a comma and no spaces.

### External Iceberg Table System Columns

* **DATA** - Column added for deployment but column is not added to iceberg table as columns specifications are not required for External Iceberg tables.
  
#### External Iceberg Table With Task Deployment Parameters

* **targetTaskWarehouse**: Alows you to specify a different warehouse used to run a task in different environments. Default value: `DEV ENVIRONMENT`

When set to `DEV ENVIRONMENT` the value entered in the **Task Scheduling Options > Select Warehouse on which to run the task** will be used when creating the task.

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

### External Iceberg Table Initial Deployment

When deployed for the first time into an environment the External Iceberg Table node will execute three stages dependent on whether or not the task schedule relies on a predecessor task.

#### External Iceberg Table No Predecessor Task Deployment

1. **Create Iceberg Table**: This stage will execute a `CREATE` OR `REPLACE` statement and create a Iceberg Table in the target environment.
2. **Create Task**: This stage will create a task that will load the target table on the schedule specified.
3. **Resume Task**: After the task has been created it needs to be resume so that the task runs on the schedule.

#### External Iceberg Table Predecessor Task Deployment

1. **Create Iceberg Table**: This stage will execute a `CREATE` OR `REPLACE` statement and create a Iceberg Table in the target environment.
2. **Suspend Root Task**: To add a task into a DAG of task the root task needs to be put into a suspended state.
3. **Create Task**: This stage will create a task that will load the target table on the schedule specified.

If a task is part of a DAG of tasks, the DAG needs to include a node type called **Task Dag Resume Root**. This node will resume the root node once all the dependent tasks have been created as part of a deployment.

The task node has no ALTER capabilities. All task enabled nodes are CREATE OR REPLACE only though this is subject to change.

### External Iceberg Table Redeployment

If any changes in config options like external volume, base location, node properties, or column results in recreating the Iceberg table during redeployment.

#### Recreating the External Snowflake Iceberg Table

* **Create Iceberg Table**: This stage will execute a `CREATE` OR `REPLACE` statement and create a Iceberg Table in the target environment.
  
#### External Iceberg Table Recreating the Task Redeployment

After the Task has deployed for the first time into a target environment, subsequent deployments with changes in task schedule, warehouse, or scheduling options will result in a `CREATE TASK `AND `RESUME TASK` statements being issued

The following stages are executed:

#### External Iceberg Table No Predecessor Task Redeployment

1. **Create Task**: This stage will create a task that will load the target table on the schedule specified.
2. **Resume Task**: After the task has been created it needs to be resume so that the task runs on the schedule.

#### External Iceberg Table Predecessor Task Redeployment

1. **Suspend Root Task**: To add a task into a DAG of task the root task needs to be put into a suspended state.
1. **Create Task**: This stage will create a task that will load the target table on the schedule specified/

### External Iceberg Table Dropped

If a Snowflake iceberg table with task is dropped from the workspace and commited to Git results in table and task dropped from target environment.

This is executed in the below stages:

#### External Iceberg Table No Predecessor Task Dropped

1. **Drop Iceberg Table**
2. **Drop Current Task**: This stage will drop the task.

#### External Iceberg table Predecessor Task Dropped

1. **Drop Iceberg Table**
2. **Suspend Root Task**: To drop a task from a DAG of task the root task needs to be put into a suspended state.
3. **Drop Task**: This stage will drop the task.

If a task is part of a DAG of tasks the DAG needs to include a node type called **Task Dag Resume Root**. This node will resume the root node once all the dependent tasks have been created as part of a deployment.

<h1 id="code">Code</h1>

<h2 id="copy-into-iceberg-table">Copy-Into Iceberg table</h2>

    Copy-Into Iceberg table node creates an Iceberg table where data is loaded from files in external stage using Copy-Into.

       An Iceberg table uses the Apache Iceberg open table format specification, which provides an abstraction layer on data files stored in open formats. [Iceberg tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) for Snowflake combine the performance and query semantics of regular Snowflake tables with external cloud storage that you manage. They are ideal for existing data lakes that you cannot, or choose not to, store in Snowflake.
       
### Copy-Into Iceberg Table Prerequisites

    * The Role in the Workspace and Environment properties of Coalesce should be `ACCOUNTADMIN` inorder to successfully create an  Iceberg table. You can also grant `SYSADMIN` roles to `EXTERNAL VOLUME`, `CATALOG INTEGRATION`created.
    * An `EXTERNAL VOLUME`, `CATALOG INTEGRATION` is expected to be created in Snowflake at the Storage Location chosen in the Node properties.}   -- To be changed

### Copy-Into Iceberg Table Configuration

The Copy-Into Iceberg table has five configuration groups:

* [Copy-Into Iceberg Table Node Properties](#copy-into-iceberg-table-node-properties)
* [Copy-Into Iceberg Table Iceberg Options](#copy-into-iceberg-table-iceberg-options)
* [Copy-Into Iceberg Table Source Data](#copy-into-iceberg-table-source-data)
* [Copy-Into Iceberg Table File Format](#copy-into-iceberg-table-file-format)
* [Copy-Into Iceberg Table Copy Options](#copy-into-iceberg-table-copy-options)

<h3 id="copy-into-iceberg-table-node-properties">Copy-Into Iceberg Table Node Properties</h3>

There are four configs within the **Node Properties** group.

* **Storage Location(required)**: Storage Location where the target table will be created.
* **Node Type(required)**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled(required)**:
  * If TRUE the node will be deployed or redeployed when changes are detected.
  * If FALSE the node will not be deployed or the node will be dropped during redeployment.

<h3 id="copy-into-iceberg-table-iceberg-options">Copy-Into Iceberg Table Iceberg Options</h3>

* **Type of catalog**: Specify the type of catalog.
       * Snowflake
       * Polaris
* **Snowflake EXTERNAL VOLUME name**: Specifies the identifier (name) for the external volume where the Iceberg table stores its metadata files and data in Parquet format. [External volume](https://docs.snowflake.com/sql-reference/sql/create-external-volume) needs to be created in Snowflake as a prerequisite.
* **Catalog integration**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.This option is enbled if the type of catalog is Polaris
* **Base location name**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.
* **Cluster Key**: Cluster key Toggle while Enabled allows us to create subset of columns in a table that are explicitly designated to co-locate the data in the table in the same micro-partitions
  * **Allow Expressions in Cluster Key**: Toggle while Enabled allows you to write expressions 
  
<h3 id="copy-into-iceberg-table-source-data">Copy-Into Iceberg Table Iceberg Options</h3>

    {![Copy-Into File location](https://github.com/user-attachments/assets/3f95f8d0-3c23-4a46-8242-fb81989c523b)} -- to be changed

* **Internal or External Stage**
  * **Coalesce Storage Location of Stage**: A storage location in Coalesce where the stage is located.
  * **Stage Name (Required)**: Internal or External stage where the files containing data to be loaded are staged
  * **File Name(s)(Optional - Ex:'a.csv','b.csv')**: Specifies a list of one or more files names (separated by commas) to be loaded
  * **File Pattern (Optional - Ex:'.*hea.*[.]csv')**: A regular expression pattern string, enclosed in single quotes, specifying the file names or paths to match.
  
        {![CopyInto-file location2](https://github.com/user-attachments/assets/0c5949c4-0286-4250-91cf-38325e2c312a)}-- to be changed

* **External location**
  * **External URI**: Enter the URI of the External location
  * **Storage Integration**: Specifies the name of the storage integration used to delegate authentication responsibility for external cloud storage to a Snowflake identity and access management (IAM) entity

<h3 id="copy-into-iceberg-file-format"> CopyInto Iceberg - File Format </h3>

    {![copy-into-iceberg-file-format](https://github.com/user-attachments/assets/2a737c5f-3bb3-471a-9365-bf3251677415)}--to be changed

* **File Format Definition - File Format Name**:
  * **File Format Name**: Specifies an existing named file format to use for loading data into the table.
  * **Coalesce Storage Location of File Format**:Location in Coalesce pointing to the database and schema,the file format resides.
  * **File Type**:
    * CSV
    * JSON
    * ORC
    * AVRO
    * PARQUET
    * XML
* **File Format Definition - File Format Values**: 
  * **File Format Values** -Provides file format options for the File Type chosen.
  * **File Type**: Each file type has different configurations available.
    * **CSV**
    * **Compression**: String (constant) that specifies the current compression algorithm for the data files to be loaded.
    * **Record delimiter**:Characters that separate records in an input file
    * **Field delimiter**:One or more singlebyte or multibyte characters that separate fields in an input file
    * **Field optionally enclosed by**:Character used to enclose strings
    * **Number of header lines to skip**:Number of lines at the start of the file to skip.
    * **Skip blank lines**:Boolean that specifies to skip any blank lines encountered in the data files.
    * **Trim Space**: Boolean that specifies whether to remove white space from fields.
    * **Replace invalid characters**: Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **Date format**:String that defines the format of date values in the data files to be loaded. 
    * **Time format**: String that defines the format of time values in the data files to be loaded
    * **Timestamp format**:String that defines the format of timestamp values in the data files to be loaded.
    * **JSON**
      * **Compression**: String (constant) that specifies the current compression algorithm for the data files to be loaded.
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
      * **Trim Space** - Boolean that specifies whether to remove white space from fields.
      * **Strip Outer Array**:Boolean that instructs the JSON parser to remove outer brackets [ ].
      * **Date format**:String that defines the format of date values in the data files to be loaded. 
      * **Time format**:String that defines the format of time values in the data files to be loaded
      * **Timestamp format**: String that defines the format of timestamp values in the data files to be loaded.
    * **ORC**
      * **Trim Space** - Specifies whether to remove white space from fields
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **AVRO**
      * **Trim Space** - Boolean that specifies whether to remove white space from fields.
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **PARQUET**
      * **Trim Space** - Boolean that specifies whether to remove white space from fields.
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **XML**
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
      

<h3 id="copy-into-iceberg-copy-options"> Copy-Into Iceberg Copy Options </h3>

* **On Error Behavior**:String (constant) that specifies the error handling for the load operation.
  * CONTINUE
  * SKIP_FILE
  * SKIP_FILE_num
  * SKIP_FILE_num%
  * ABORT_STATEMENT
* **Specify the number of errors that can be skipped**: Required when On Error Behavior is either `SKIP_FILE_num` or `SKIP_FILE_num%`. Specify the number of errors that can be skipped.
* **Size Limit**: Number (> 0) that specifies the maximum size (in bytes) of data to be loaded for a given COPY statement.
* **Purge Behavior**: Boolean that specifies whether to remove the data files from the stage automatically after the data is loaded successfully.
* **Return Failed Only**: Boolean that specifies whether to return only files that have failed to load in the statement result.
* **Force**: Boolean that specifies to load all files, regardless of whether they’ve been loaded previously and have not changed since they were loaded. 
* **Load Uncertain Files**: Boolean that specifies to load files for which the load status is unknown. The COPY command skips these files by default.
* **Enforce Length**: Boolean that specifies whether to truncate text strings that exceed the target column length
* **Truncate Columns**: Boolean that specifies whether to truncate text strings that exceed the target column length

### Copy-Into Iceberg System Columns

The set of columns which has source data and file metadata information.

* **SRC** - The data from the file is loaded into this variant column.
* **LOAD_TIMESTAMP** - Current timestamp when the file gets loaded.
* **FILENAME** - Name of the staged data file the current row belongs to. Includes the full path to the data file.
* **FILE_ROW_NUMBER** - Row number for each record in the staged data file.
* **FILE_LAST_MODIFIED** - Last modified timestamp of the staged data file the current row belongs to
* **SCAN_TIME** - Start timestamp of operation for each record in the staged data file. Returned as TIMESTAMP_LTZ.


### Copy-Into Iceberg Deployment

#### Copy-Into Iceberg Deployment Parameters

The Copy-Into Iceberg includes an environment parameter that allows you to specify if you want to perform a full load or a reload based on the load type when you are performing a copy-into-iceberg operation.

The parameter name is `loadType` and the default value is ``.

```json
{
    "loadType": ""
}
```

When the parameter value is set to `Reload`, the data is reloaded into the table regardless of whether they’ve been loaded previously and have not changed since they were loaded.

#### Copy-Into Iceberg Initial Deployment
When deployed for the first time into an environment the copy-into Iceberg node of materialization type table will execute the below stage:

**Create table/transient table**
This will execute a CREATE OR REPLACE statement and create a table or transient table in the target environment.


### Copy-Into Iceberg Redeployment

#### Altering the Copy-Into Iceberg Tables

There are few column or table changes like Change in table name,Dropping existing column, Alter Column data type,Adding a new column if made in isolation or all-together will result in an Create statement to modify the Work Table in the target environment.

The following stages are executed:

* **Create Iceberg table**: Create table statement is executed to perform the alter operation.


### Copy-Into Iceberg Undeployment

If the Copy-Into Iceberg node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher-level environment then the target table in the target environment will be dropped.

* **Drop table/transient table**: Target table in Snowflake is dropped


<h2 id="snowpipe-iceberg-table">Snowpipe Iceberg Table</h2>

   The Coalesce Snowpipe Iceberg node is a node that creates an Iceberg table and performs two operations. It can be used to load historical data using     
     [CopyInto](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table). Also the Snowpipe node can be used to create a pipe to auto ingest files from AWS, GCP, or Azure.

    [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro) enables loading data from files as soon as they’re available in a stage. 

    This means you can load data from files in micro-batches, making it available to users within minutes, rather than manually executing COPY statements on a schedule to load larger batches.

### Snowpipe Iceberg Node Configuration

The Snowpipe Iceberg node type has five configurations groups:

* [Snowpipe Iceberg Node Properties](#snowpipe-iceberg-node-properties)
* [Snowpipe Iceberg Options](#snowpipe-iceberg-options)
* [Snowpipe Iceberg Snowpipe Options](#snowpipe-iceberg-snowpipe-options)
* [Snowpipe Iceberg File Location](#snowpipe-iceberg-file-location)
* [Snowpipe Iceberg File Format](#snowpipe-iceberg-file-format)
* [Snowpipe Iceberg Copy Options](#snowpipe-iceberg-copy-options)

<h3 id="snowpipe-iceberg-node-properties">Snowpipe Iceberg Node Properties</h3>

* **Storage Location**: Storage Location where the target table will be created.
* **Node Type**: Name of template used to create node objects.
* **Description**: A description of the node's purpose.
* **Deploy Enabled**:
  * If TRUE the node will be deployed / redeployed when changes are detected.
  * If FALSE the node will not be deployed or will be dropped during redeployment.

<h3 id="snowpipe-iceberg-snowpipe-options"> Options </h3>

* **Enable Snowpipe**: Drop down that helps us to create a pipe to auto ingest files from external stage or validate the Copy-Into statement.
  * Enable Snowpipe - Load data auto ingesting files from AWS, Azure, or GCP. Choose your **Cloud Provider.**
    * AWS - AWS SNS Topic. Specifies the Amazon Resource Name (ARN) for the SNS topic for your S3 bucket.
    * Azure - Integration. Specifies the existing notification integration used to access the storage queue.
    * GCP -  Integration. Specifies the existing notification integration used to access the storage queue.
  * Test Copy Statement - To validate the Copy-into statement before we use it to create PIPE
* **Load historical data**:Loads the historic data into the target table by executing a COPY_INTO statement.
  
<h3 id="snowpipe-iceberg-table-iceberg-options">Snowpipe Iceberg Table Iceberg Options</h3>

* **Type of catalog**: Specify the type of catalog.
       * Snowflake
       * Polaris
* **Snowflake EXTERNAL VOLUME name**: Specifies the identifier (name) for the external volume where the Iceberg table stores its metadata files and data in Parquet format. [External volume](https://docs.snowflake.com/sql-reference/sql/create-external-volume) needs to be created in Snowflake as a prerequisite.
* **Catalog integration**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.This option is enbled if the type of catalog is Polaris
* **Base location name**: Specifies the identifier (name) of the [catalog integration](https://docs.snowflake.com/user-guide/tables-iceberg#label-tables-iceberg-catalog-integration-def) for this table.
* **Cluster Key**: Cluster key Toggle while Enabled allows us to create subset of columns in a table that are explicitly designated to co-locate the data in the table in the same micro-partitions
  * **Allow Expressions in Cluster Key**: Toggle while Enabled allows you to write expressions 

<h3 id="snowpipe-iceberg-file-location"> File Location </h3>

* **Coalesce Stage Storage Location of stage(Required)**: A storage location in Coalesce where the stage is located.
* **Stage Name (Required)**: Internal or External stage where the files containing data to be loaded are staged.
* **File Names**: Enabled when 'Enable Snowpipe' under Snowpipe Options is toggled off. Specifies a list of one or more files names (separated by commas) to be loaded. For example, `'a.csv','b.csv'`.
* **File Pattern**:A regular expression pattern string, enclosed in single quotes, specifying the file names or paths to match. For example, `*hea.*[.]csv'`.
  
<h3 id="snowpipe-iceberg-file-format"> File Format </h3>

* **File Format Definition - File Format Name**:
  * **File Format Name**: Specifies an existing named file format to use for loading data into the table.
  * **Coalesce Storage Location of File Format**: File format location in snowflake that is mapped as storage location in Coalesce
  * **File Type**:
    * CSV
    * JSON
    * ORC
    * AVRO
    * PARQUET
    * XML
* **File Format Definition - File Format Values**: 
  * **File Format Values** -Provides file format options for the File Type chosen.
  * **File Type**: Each file type has different configurations available.
    * **CSV**
    * **Compression**: String (constant) that specifies the current compression algorithm for the data files to be loaded.
    * **Record delimiter**:Characters that separate records in an input file
    * **Field delimiter**:One or more singlebyte or multibyte characters that separate fields in an input file
    * **Field optionally enclosed by**:Character used to enclose strings
    * **Number of header lines to skip**:Number of lines at the start of the file to skip.
    * **Skip blank lines**:Boolean that specifies to skip any blank lines encountered in the data files.
    * **Trim Space**: Boolean that specifies whether to remove white space from fields.
    * **Replace invalid characters**: Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **Date format**:String that defines the format of date values in the data files to be loaded. 
    * **Time format**: String that defines the format of time values in the data files to be loaded
    * **Timestamp format**:String that defines the format of timestamp values in the data files to be loaded.
    * **JSON**
      * **Compression**: String (constant) that specifies the current compression algorithm for the data files to be loaded.
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
      * **Trim Space** - Boolean that specifies whether to remove white space from fields.
      * **Strip Outer Array**:Boolean that instructs the JSON parser to remove outer brackets [ ].
      * **Date format**:String that defines the format of date values in the data files to be loaded. 
      * **Time format**:String that defines the format of time values in the data files to be loaded
      * **Timestamp format**: String that defines the format of timestamp values in the data files to be loaded.
    * **ORC**
      * **Trim Space** - Specifies whether to remove white space from fields
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **AVRO**
      * **Trim Space** - Boolean that specifies whether to remove white space from fields.
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **PARQUET**
      * **Trim Space** - Boolean that specifies whether to remove white space from fields.
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.
    * **XML**
      * **Replace invalid characters** - Boolean that specifies whether to replace invalid UTF-8 characters with the Unicode replacement character.

<h3 id="snowpipe-iceberg-copy-options"> Copy Options </h3>

    {![snowpipe-iceberg-copy-options](https://github.com/user-attachments/assets/ae640901-16ce-4ed2-8e6c-efae278d3942)} 

If you toggle Enable Snowipe under Snowpipe Options to *ON*, these configuration options are available.

* **On Error Behavior**: String (constant) that specifies the error handling for the load operation.
  * CONTINUE
  * SKIP_FILE
  * SKIP_FILE_num
    * Specify the number of errors that can be skipped.
  * SKIP_FILE_num%
    * Specify the number of errors that can be skipped.
* **Enforce Length**: Boolean that specifies whether to truncate text strings that exceed the target column length.
* **Truncate Columns**: Boolean that specifies whether to truncate text strings that exceed the target column length.

If you toggle Enable Snowpipe under Snowpipe Options to *OFF*, these configuration options are available.

* **On Error Behavior**:String (constant) that specifies the error handling for the load operation.
  * CONTINUE
  * SKIP_FILE
  * SKIP_FILE_num
  * SKIP_FILE_num%
  * ABORT_STATEMENT
* **Specify the number of errors that can be skipped**: Required when On Error Behavior is either `SKIP_FILE_num` or `SKIP_FILE_num%`. Specify the number of errors that can be skipped.
* **Size Limit**: Number (> 0) that specifies the maximum size (in bytes) of data to be loaded for a given COPY statement.
* **Purge Behavior**: Boolean that specifies whether to remove the data files from the stage automatically after the data is loaded successfully.
* **Return Failed Only**: Boolean that specifies whether to return only files that have failed to load in the statement result.
* **Force**: Boolean that specifies to load all files, regardless of whether they’ve been loaded previously and have not changed since they were loaded. 
* **Load Uncertain Files**: Boolean that specifies to load files for which the load status is unknown. The COPY command skips these files by default.
* **Enforce Length**: Boolean that specifies whether to truncate text strings that exceed the target column length
* **Truncate Columns**: Boolean that specifies whether to truncate text strings that exceed the target column length

### Snowpipe Iceberg System Columns

The set of columns which has source data and file metadata information.

* **SRC** - The data from the file is loaded into this string column.
* **LOAD_TIMESTAMP** - Current timestamp when the file gets loaded.
* **FILENAME** - Name of the staged data file the current row belongs to. Includes the full path to the data file.
* **FILE_ROW_NUMBER** - Row number for each record in the staged data file.
* **FILE_LAST_MODIFIED** - Last modified timestamp of the staged data file the current row belongs to
* **SCAN_TIME** - Start timestamp of operation for each record in the staged data file. Returned as TIMESTAMP_LTZ.

### Snowpipe Iceberg Deployment

#### Snowpipe Iceberg Deployment Parameters

The Snowpipe includes two environment parameters.The parameter `loadType` allows you to specify if you want to perform a full load or a reload based on the load type 
when you are performing a Copy-Into operation.
The another parameter `targetIntegrationOrAwsSNSTopic` allows you to specify the AWS SNS Topic or Integration name to be used for auto ingesting files from Clous providers

The parameter name is `loadType` and the default value is ``.

```json
{
    "loadType": ""
}
```

When the parameter value is set to `Reload`, the data is reloaded into the table regardless of whether they’ve been loaded previously 
and have not changed since they were loaded.

The parameter name is targetIntegrationOrAwsSNSTopic and the default value is DEV ENVIRONMENT.

When set to DEV ENVIRONMENT the value entered in the Snowpipe Options config AWS SNS TOpic/Integration is used for ingesting files from Cloud providers.

```
{
    "targetIntegrationOrAwsSNSTopic": "DEV ENVIRONMENT"
}
```

When set to any value other than DEV ENVIRONMENT the node will use the specified value for AWS SNS TOpic/Integration.

For example, the Snowpipe node will use the specific value for auto ingestion.

```
{
    "targetIntegrationOrAwsSNSTopic": "arn:aws:sqs:us-east-1:832620633027:sf-snowpipe-AIDA4DXAOTPB7IF437EPD-lZe8ciYpPqGgC9KwWLmiIQ"
}
```

#### Snowpipe Iceberg Initial Deployment

When deployed for the first time into an environment the Snowpipe node will execute the below stages depending on if Enable Snowpipe is enabled,'Load Historical Data' is enabled and the `loadType` parameter.

| Deployment Behavior  | Enable Snowpipe | Historical Load | Load Type | Stages Executed |
|--|--|---|--|--|
| Initial Deployment | Enable Snowpipe |true| ``|Create Iceberg Table </br> Historical full load using CopyInto </br> Create Pipe </br> Alter Pipe 
| Initial Deployment | Enable Snowpipe |true| Reload|Create table </br> Truncate Target table </br> Historical full load using CopyInto </br> Create Pipe </br> Alter Pipe
| Initial Deployment | Enable Snowpipe |false| Reload or Empty|Create Iceberg table </br> Truncate Target table </br> Create Pipe
| Initial Deployment | Test Copy Statement |false| Reload or Empty|Create Iceberg table </br> Test Copy Statement-No pipe creation

### Snowpipe Iceberg Redeployment

#### Altering the Snowpipe Iceberg node

There are few column or table changes like Change in table name, Dropping existing column,  Alter Column data type, Adding a new column if made in isolation or all-together will result in an Create statement to modify the target Table in the target environment.Any table level changes or node config changes results in recreation of pipe

The following stages are executed:

* **Create Iceberg table|**: Create table statement is executed to perform the alter operation.
* **Truncate target table**:The target table is truncated in case the Load Type parameter is set to 'Reload'
* **Historical full load using CopyInto**:Historical data are loaded if 'Load Historical' toggle is on.
* **Create Pipe**: Pipe is recreated if enable snowpipe option is true
* **Alter Pipe**:Pipe is refreshed if 'Load Historical' toggle is on.

### Snowpipe Iceberg Undeployment

If the Snowpipe node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher-level environment then the target table in the target environment will be dropped.

This is executed in two stages:

* **Drop Table**: Target table is dropped
* **Drop Pipe**: Pipe is dropped

## Snowflake Iceberg table

* [Node definition](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/SnowflakeIcebergtable-305/definition.yml)
* [Create Template](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/SnowflakeIcebergtable-305/create.sql.j2)

## External Iceberg table

* [Node definition](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/ExternalIcebergtable-307/definition.yml)
* [Create Template](https://github.com/coalesceio/Iceberg-tables/blob/main/nodeTypes/ExternalIcebergtable-307/create.sql.j2)
