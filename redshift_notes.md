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





