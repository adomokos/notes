## Redshift Notes

Redshift Cluster - could have 1 to many DBs on it, up-to 60

The cluster is made up of separate nodes.
Two node types - leader (always 1) and compute nodes.

The leader:
* client interaction
* execution plan \ optimization
* instruction compilation
* work distribution

Compute nodes:
* dedicated OS CPUs, memory and storage
* Node scalability
* Dense storage \ dense compute
* Node slices

### Cluster Setup

Dense compute vs Dense storage

dc2.large - for proof of concept
You can set up the compute nodes.
Use jdbc or odbc to connect to the Redshift cluster.

### Redshift Pricing

Two approaches:
* on demand (pay by the hour)
* reserved instance pricing
* Redshift Spectrum?

dc2.large - $2,190 per node for a year
$13,687.5 per TB

https://aws.amazon.com/redshift/pricing/

### Redshift Query Optimization

* query optimization
* internal stats
* ACID copmliance
* Data distribution

### Distributation Styles

The goal of selecting a distribution style is minimizing impact of redistribuation step.

Critical to choose the right distribution style.
The distribution style should match the schema we choose.

There are 3 distribution styles:
1. Even Distribution
2. Key Distribution
3. All Distribution

#### Even Distribution
* Distributes values evenly
* Default distribution style
* Good to average performance

#### Key Distribution
* Rows distributed by selected key
* Known relationships distributed across the same nodes
* Matching data is in one cluster, no cross query is needed.

#### All Distribution
* All rows are copied to all nodes
* Significant increase to storage and cost of insert, updates and deletes
* Best for medium size tables that change rarely and actively joined (like Dates Dim or Clients Dim)

### Table Setup

Upper-cased DDLs will be converted to lower-cased to follow Postgres style.

Primary keys are just helping Redshift with query optimization. It's perfectly valid to insert records with the same ID. The responsibility of enforcing foreign key relationship is happening through the ETL. cons: data integrity, pros: performance, no verification of keys.

### Column-oriented Storage

In traditional RDMSs the data is row oriented, the query has to read in all pages that has salary info to calculate the avg. The colunm-oriented storage means that it's only reading in values that are relevant to the computations.

Redshift stores its data in 1MB chunks.

* Clustered index or non-clustered index is a no-go in Redshift.
* There is a column-oriented, massively parallel, scale out architecture.
* Not optimal for CRUD operations - use RDS for that

### Redshift Sorting

* A sortkey sorts data on disk
* Declared like distkey and primary key
* Part of create statement
* Only one sortkey permitted

### Effective Compression

* Since it's column-oriented, compression is much more effective.
* Optimized for single data type
* Speeds data reads
* Reduces overhead of redistribution
* Customizable compression

### Getting Data in Redshift

* COPY is the most powerful
* Integrating Redshift into Your ETL
   - Leveraging Copy
   - Staging Tables
   - Transformations
   - Automated Loads

Loading into Nodes and Node Clices

### COPY Command

#### CSV file

* Don't be tempted with INSERT

```sql
  copy dimproduct
  -- options column list, ordinal by default
  from '' -- specify the source
  iam_role '' -- several other ways to specify authorization
  -- format, additional options
```

Loading the CSV file took 55 seconds. SUPER SLOW!!!

#### Load with manifest

* Try pipe-delimited
* leverage parallel load processing (multiples of compute clusters)
* You can leverage gzip as well
* You can set up a manifest file, all parallel load will leverage that

```sql
  copy factsales
  from 's3://address'
  region 'us-west-2'
  iam_role 'arn:aws....'
  GZIP
  delimiter '|'
  manifest
```

240,388 records were loaded this way in 1m 3s in the demo.
Across - US East -> US West

#### Load dimdate
Data is in JSON format

```sql
  copy dimdate
  from 's3://address'
  region 'us-west-2'
  iam_role 'arn:aws....'
  json as 'auto'
```
Error received:
```
[Amazon](500310) Invalid operation: Load into table 'dimdate' failed. Check 'stl_load_errors' system table for details.;
```

This table is where all the logs are.

```sql
select * from stl_load_errors
```

JSON should not be in an array, no comma between documents.
2,100 records loaded in 18 seconds.

We could load data directly from DynamoDB.
Other zip files.
We could load data from an SSH sesssion.

### Loading with ETLs

Redshift is a different platform, SISS, or other conventional tools might not apply.

### Streaming Data into Redshift with Kinesis

Delivery streaming

PUT options:
* Firehose PUT APIs
* Amazon Kinesis Agent
* AWS IoT
* CloudWatch Logs or Events

Demo used direct put.
We could create transformations with AWS Lamdba.
Destination -> AWS S3 or Redshift Cluster.

We can create test streams to make sure it's set up properly.

Lambda can be injected, it can process data real time, good alternative to stream data into the DW continuously.

## Connectoring, Querying, and Consuming Data

AMZN Redshift SQL Syntax

* ANSI SQL Standards
* Redshift quirks
* Diff between Redshift and Postgres
    - DDL diffs
    - DML diffs

### Redshift SQL Engine

* Differences between Redshift and other SQL engines
* Supported constraints are for query optimization, not data quality
* Some constraints not supported - default, check, exclusions
* "Create Index" does not exist - as Redshift is column oriented DB
* "Alter Column" does not exist
* "Alter Table" does support adding and dropping columns

Data Types:
* Smallint (Int2)
* Integer (Int, Int4)
* Bigint (Int8)
* Decimal (Numeric)
* Real (Float4)
* Double Precision (Float8, Float)
* Boolean (Bool)
* Char (Character, NChar, BPChar)
* Varchar (Character Varying, NVarChar, Text)
* Date
* Timestamp (Timestamp without Time Zone)
* Timestampz (Timestamp with Time Zone)

String manipulation:
* CONCAT()
* SUBSTRING()
* POSITION()
* REGEX_*()
    - search "Redshift String Manipulation"

Date functions:
* DATEADD()
* DATEDIFF()
* DATE_PART()
* LAST_DAY()
    - search "Redshift Date Manipulation" for more info

Math functions:
* SUM()
* ABS()
* COS()
* FLOOR()
* MOD()
  - search "Redshift Math Functions"

Data conversion functions:
* CAST()
* CONVERT()

[Redshift developer guide](https://aws.amazon.com/documentation/redshift)

### Txn, Isolation, Select

SELECT is full featured
CTEs are also supported

### Ranking Window Functions

* DENSE_RANK
* NTILE
* PERCENT_RANK
* RANK
* ROW_NUMBER

### Advanced Queries

We can create Python functions:

```sql
create or replace function month_name(n integer)
return varchar(3)

stable as $$

if n==1:   return 'Jan'
elif n==2: return 'Feb'
...

$$ language plpythonu;
```

Building views, that can be used by other BI tools.

## Redshift Security

OSI - Open System Interconnection
* Physical layer
* Data layer
* Network layer
* Application layer

### Shared Responsibility

Amazon AWS Shared Responsibility

Responsibility of AWS:
* Secrurity of host OS and down
* Physical security of data centers
* Upkeep of infrastructure
* Redundancies, SLAs, patching

Responsibility of Client:
* Security of guest operating system
* Security of applications and platform
* Security of VPC
* Security of Data

### Securing the VPC

* Similar with physical network
* Get training or hire a professional

### Root-account Policy
* Locked away physically and forgotten
* Used once to establish security account delegated areas of control

### Redshift Data Loading Lifecycle

* Source files
* S3 storage
* Redshift cluster

Server-side encryption:
Data can be encrypted
S3 encrypts the data it receives, then decrypts it to transform it and send it over to Redshift in an encrypted format.
Request S3 to encrypt data by providing the encryption header in the REST call.
We can request encryption in AWS management console

Client side encryption:
Encrypt data before sent to S3.

```sql
copy dimproduct from 's3://mybucket/...'
iam_role 'arn:aws:iam::...'
master_symmetric_key '<master_key>'
encrypted
delimiter '|';
```
SSL is always recommended!
SSL is always enabled, but not required by default.
Set `require_ssl` false to true.

Redshift is using AWS Hardware Accelerated SSL
We don't need to worry about protecting over the wire.

### AMZN Redshift Security

User security:
* Similar to other RDBMS engines
* Management of GRANT and REVOKE
* USERS are grouped into GROUPS
* Objects

GRANT gives right to perform a function on an object to a user or to a group.

* Users receive no rights by default
* SUPERUSER has all rights
* The number of SUPERUSER should be limited

The Demo DW User Groups are the followings:

* Finance
* Sales
* Data Warehouse Developers

```sql
-- list of finance users
create user joesmith with password 'somePassword1';
create user janesmith with password 'somePassword1';
create user financeapp with password 'appPassword1';

create group finance;

alter group finance add user joesmith;
alter group finance add user janesmith;

grant select on table dimproduct to group finance;
grant select on table factsales to group finance;

grant update on table factsales to financeapp;
```

Schema's can be a group of tables, schemas can have special security:

```sql
create schema secure;

create table secure.accountnum
(accountkey int,
 accountnum varchar(10),
 accountloc varchar(23),
 payoutcode varchar(15)
 );

 create table secure.accountholder
 (accountkey int,
  account_name varchar(50),
  account_contact varchar(25));

grant usage on schema "secure" to group finance;

-- this would grant select to all existing tables, but not new ones
grant select on all tables in schema "secure" to group finance;

-- this would grant select on all future tables as well
alter default privileges in schema "secure" grant select on tables to group finance;
```

Grant select columns to a group from a table, by using a view. Then that view can have select access to a group.

## Maintenance

### Backup

Redshift maintains 3 copies of your cluster:
* Original cluster
* Cluster node replicas
* Backup on S3 Storage

AWS Provides an equal amount of storage for backup as your configured cluster

(By default, a single day backup is automatically taken and stored to S3.)

### Vacuum

When Redshift deletes, it does not reclaim space. Over time these could add additional query time.

You can run VACUUM on the cluster or on the tables.

VACUUM complete cluster requires superuser

```sql
select * from svv_vacuum_progress`;
```

### System Views

Provides a glimpse into the inner-workings of RedShift.
`stl` - system table logs
`stv` - system table snapshots
`svv` - system view snapshot
`svl` - system view log
`pg` - catalog tables

### A glance of system views

https://github.com/awslabs/amazon-redshift-utils

Good code snippets, tutorials.

EXPLAIN command is very helpful to understand query plans.

