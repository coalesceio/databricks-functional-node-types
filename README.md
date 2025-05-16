## Functional Node Types

The Coalesce Functional Node Types Package includes:

* [Date Dimension](#date-dimension)
* [Code](#code)

---

## Date Dimension

The Coalesce Date Dimension Table provides a comprehensive breakdown of date-related attributes, enabling efficient handling of date operations across various
use cases. The table typically includes columns such as day, month, year, day of the week, week of the year, quarter, and flags like day is weekday
or weekend. Additional columns like fiscal year, fiscal quarter, holiday indicators can also be included, depending on the requirements.

### Date Dimension Node Configuration

The Date Dimension node type has two configuration groups:

* [Node Properties](#date-dimension-node-properties)
* [Options](#date-options)
* [Additional options](#additional-options)

![Fact_config](https://github.com/coalesceio/Coalesce-Base-Node-Types---Advanced-Deploy/assets/7216836/45d22ea5-32ca-49f5-a464-b266cb29b516)

#### Date Dimension Node Properties

| **Setting** | **Description** |
|----------|-------------|
| **Storage Location** | Storage Location where the WORK will be created |
| **Node Type** | Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/> If FALSE the node will not be deployed or will be dropped during redeployment |

##### Date Options

| **Setting** | **Description** |
|---------|-------------|
| **Starting Date**| A date from where the date values should be added in the date table.Default is :DATEADD(DAY, -730, CURRENT_DATE)|
| **Number of Days To Generate** | Numeric value indicating how many days' records should be generated from the Starting Date. |
| **Generated Date Column Name** |Metadata column name used in the SQL generated for inserting records into the table. |

#### Additional Options

You can create the node as:

* [Table](#date-table-create-as-table)
* [View](#date-table-create-as-view)

##### Date Dimension Create as Table

| **Setting** | **Description** |
|---------|-------------|
| **Create As**| Table|
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes. **True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Insert Zero Key Record** | Toggle: True/False<br/>Insert Zero Key Record to Dimention if enabled |
| **Business key** | Required column for Type 1 Dimensions |
| **Default String Value** | If Insert Zero Key Record toggle is True then add a default value for columns with datatype string |
| **Default Surrogate Key Value** | If Insert Zero Key Record toggle is True then add a default value for surrogate key column|
| **Default Date Value (Date Format DD-MM-YYYY)** | If Insert Zero Key Record toggle is True then add a default value for date key column in the format DD-MM-YYYY|
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Pre-SQL**| SQL to execute before data insert operation |
| **Post-SQL** | SQL to execute after data insert operation |

##### Date Dimension Create as View

| **Setting** | **Description** |
|---------|-------------|
| **Create As**| View|
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Override Create SQL** | Toggle: True/False<br/>**True**: View is created by overriding the SQL<br/>**False**: Nodetype defined create view SQL will execute |

### Date Dimension Joins

Join conditions and other clauses can be specified in the join space next to mapping of columns in the UI.

![work_join](https://github.com/coalesceio/Coalesce-Base-Node-Types---Advanced-Deploy/assets/7216836/7acff10b-8845-4c44-851a-b7a0bc7eaf41)

> 📘 **Specify Group by and Order by Clauses**
>
> Best Practice is to specify group by and order by clauses in this space if you are not opting for the group by all and order by provided in OPTIONS config.

### Date Dimension Deployment

#### Date Dimension Initial Deployment

When deployed for the first time into an environment the Date node of materialization type table or view will execute the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Date Table** | This will execute a CREATE OR REPLACE statement and create a table in the target environment |
| **Create Date View** | This will execute a CREATE OR REPLACE statement and create a view in the target environment |

#### Date Dimension Redeployment

After the Date node with materialization type table/view has been deployed for the first time into a target environment, subsequent deployments may result in either altering the Date Table/view or recreating the Date table/view.

#### Altering the Date Table 

A few types of column or table changes will result in an ALTER statement to modify the Persistent Table in the target environment, whether these changes are made individually or all together:

1. Changing table names
2. Dropping existing columns
3. Altering column data types
4. Adding new columns

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Rename Table/ Alter Column/ Delete Column/ Add Column/Edit table description** | Alter table statement is executed to perform the alter operation |

#### Date Dimension Recreating the Views

The subsequent deployment of Date node of materialization type view with changes in view definition, adding table description or renaming view results in deleting the existing view and recreating the view.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Create View** | Creates a new view with updated definition |

#### Date Dimension Drop and Recreate View/Table/Transient Table

| **Change** | **Stages Executed** |
|------------|-------------------|
| **View to table** |  Drop view <br/> Create or Replace Date table/transient table |
| **Table to View** |  Drop table/transient table<br/> Create Date view |

> 📘 **Materialization Date Dimension**
>
> When the materialization type of Date node is changed from table to View and use Override Create SQL for view creation. This ensures that the following change is made in the stage function in Create SQL tab so that the order of deployment is maintained.

![CreateSQL](https://github.com/coalesceio/Coalesce-Base-Node-Types---Advanced-Deploy/assets/7216836/0296abf8-0747-4ae8-8478-0782e5e2e545)

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment,then no stages are executed

### Date Dimension Deploy Undeployment

If a Date Dimension Node of materialization type table/view are deleted from a Datespace, that Datespace is committed to Git and that commit deployed to a higher level environment then the DateTable in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/view** | Removes the table or view from the environment |

## Code

### Date Table Code

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/databricks-functional-node-types/blob/main/nodeTypes/DateDimension-461/create.sql.j2)|
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/databricks-functional-node-types/blob/main/nodeTypes/DateDimension-461/definition.yml) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/databricks-functional-node-types/blob/main/nodeTypes/DateDimension-461/run.sql.j2) |

