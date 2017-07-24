class: center, middle

# CH7 - Databases and AWS

---
# SQL Databases

  - Relational databases
    - PostgreSQL, MSSQL, ORACLE,
    - Tables, Referential integrity, Consistency
    - Structured Query Language
    - OLAP (Analytical, DataWarehouses) or OLTP (Transactional)
  - Amazon RDS
  - OLAP
    - Used for reporting, Analysis
    - High Query Complexity
    - Low Query Frequency
    - Batch Processing
  - OLTP
    - Main production database
    - Many IO operations per second
    - Not-so-complex queries
  - RDS is iften used for OLTP workloads, but it can be used for OLAP
  - Redshift is a data warehouse designed specially for OLAP

---
# NoSQL Databases


  - Simpler to use
  - More flexible
  - Better performance than a RDBMS
  - Horizontal scalability at a fraction of the cost
  - Non-relational! (no tables, no sql)
  - Key/value-based or document-based (generally) that can evolve easily
  - Large volumes of data with high transaction rates
  - MongoDB, HBase, Cassandra, CouchDB, Amazon DynamoDB
  - You can deploy your own on an EC2 or use AWS DynamoDB
---
# Amazon Relational Database Service

  - Managed DB service
  - Focus on the app and schema and let AWS handle backup, patching, scaling and replication
  - Provides an enpoint that you can use to work with the database using SQL
  - Amazon doesn't provide shell access to the instance
  - Access to high-privileged procedures and system tables is restricted
  - Usually all it takes to work with RDS is changing the connection string
  - It is handled in DB instances
  - Provides an API
  - Computational power is handled by instance types (can change over time)
---
# Database Engines

  - MySQL - MultiAZ deployments, Horizontal Scaling with Read Replicas
  - PostgreSQL -  MultiAZ deployments, Horizontal Scaling with Read Replicas
  - Oracle - MultiAZ on every version/edition
  - MSSQL server -  MultiAZ only onthe standard and enterprise editions
  - Aurora - its a defacto cluster that spans over several AZ
---
class: center, middle

# Licenses for MSSQL and Oracle can be purchased or BYOL

---

# Storage Options

  - Magnetic
  - General purpose
  - Provisioned IOPS

---

# Backup and Recovery

  - Automated Backups or Manual Snapshots
  - RPO - Maximum period of data loss
  - RTO - Maximum amount of downtime that is permitted to recover from Backup
  - Automated backups
    - Backups are done for entire instances
    - Retention period is 1 day by default, but can increase up to 35
    - When you delete the instances, these are completely lost
  - Manual DB snapshots
    - Initiated by the user
    - Aren't lost when the instance is deleted
    - Restore the DB Instance to a specific state
    - it impacts the performance while the db is being backed up
    - It is suggested to use multiAZ to reduce the impact of performing it

---

# Backup and Recovery

  - RDS allows for quick recovery of the DB
  - New DB instance is created for the recovery
  - AWS combines the daily backups with transaction logs to restore the DB instance to any point during the retention period

---

# HA with multiAZ

  - Allows to create a DB cluster across AZ
  - You can meet the most demanding RTO and RPO by using sync replication
  - A single endpoint is provided
  - A master DB is placed in one AZ and a slave in another
  - Automatically replicates the primary data to the slave DB in sync
  - Auto-failover for the most common failure scenarios

---

# Scaling Up and Out

  - Vertical Scalability - Adding more resources to a DB (Immediate/Scheduled)
    - Storage can scale from 5gb to 6tb
  - Horizontal Scalability with partitioning
    - more users and requests but requires additional logic in the app layer
  - Horizontal Scalability with read replicas
    - Offloading read transactions from the primary DB
    - For read intensive db workloads
    - Offloading reporting and data warehousing
    - Special DB Instance called replica

---

class: center, middle

# Secutiry

---

class: center, middle

# Use IAM policies!

---

class: center, middle

# Deploy the DB instance into a private subnet!

---

class: center, middle

# Restrict using security groups and ACLs

---

class: center, middle

# Use in transit and at-rest encryption for data and logs (SSL, KMS, TDE)

---

# Amazon Redshift

  - Is a fast, powerful and fully managed, petabyte-scale data warehouse service in the cloud
  - Designed for OLAP scenarios
  - It allows you to use SQL, and its based on PostgreSQL
  - The key component is a cluster, which is composed of a leader node and one or more compute nodes
  - Your app will interact only with the lead node using JDBC and ODBC, compute nodes are transparent
  - There are 6 different node types, and are divided into dense compute and dense storage
    - Dense compute nodes support 326TB using fast SSD
    - Dense storage nodes support up to 2PB using large mag disks
  - Each cluster has one or more DB
  - Your data is spread among all compute nodes in a cluster based on a distribution strat that you specify

---

# Amazon Redshift

  - You can resize your db either by changing the node types or increasing adding more compute power
  - When a resizing takes place, redshift creates a new cluster and migrates the old cluster to the new one, DB will be read only until the operation is finished
  - Table design - just like in a regular sql db
  - Data types - just like sql, but you cannot modify existing fields
  - Compression encoding - you can specify it at table creation, or redshift will choose one for you
  - Sort keys - Sorting enables efficient handilng of range restricted queries. Can be interleaved or compound

---
# Distribution strategy

  - Aimed to minimize the impact of haviong the data distributed accross nodes and slices in a cluster
  - You need to decide it at table creation time
  - The one you choose will have a big impact on query performance
  - There are 3 types:
    - EVEN - Is the default option and results in the data being distributed across the slices in a uniform fashion.
    - KEY - rows are distributed according to the values in one column. The lead node will store matching values together to increase query performance of joins
    - ALL - Full copy of the entire table is distributed to every node, good for large tables that are not updated frequently

---
# Loading data

  > You can use regular insert or update to create or modify data but for bulk actions a copy call can read from multiple files at once making it way faster when loading data from services like S3. Instead of having one large file, you can enable parallel processing by having a cluster with multiple nodes and multiple input files.

  > After a bulk data load you will need to perform a vacuum command to reorganize your data and claim the free space after it deletes. Data can also be exported using the unload command, which can generate text files and store them in s3

---
# Snapshots

  - Point in time snapshots can be created and used to restore a copy or create a clone for your redshift cluster
  - You can use automated or manual snapshots. 
  - Automated snapshots can then be stored for a configurable retention period
  - Manual snapshots can be replicated across regions or even accounts 

---

class: center, middle

# Securing a redshift cluster is similar to securing other databases running in the cloud

---


class: center, middle

# Amazon DynamoDB

---

# Amazon DynamoDB

  - Pretty much your run-off-the-mill noSQL database
  - Fully managed service handled by AWS
  - It's data model is comprised by tables, items and attributes
  - Each item will have a primary key
  - It allows complex attributes
  - No need to define the table schema past the PK
  - Apps can connect to DynamoDB via web service API (json format) or SDK

--

  - Scalar Data types:
    - String - Unicode UTF8 encoded text up to 400KB
    - Number - 38 digits of precision
    - Binary - Binary data, images, compressed objects up to 400kb
    - Boolean
    - Null
  - Set Data types:
    - String Set
    - Number Set
    - Binary Set
  - Document Data types are useful to represent nested attributes:
    - List
    - Map (Key/value pairs)

---

# Primary key

  > When a table is created, you must specify the primary key in addition to the table name. This will serve as the unique identifier of any item. 2 types of PK are supported:

  - Partition key - The unique id will be made by one attribute, a hash key.
  - Partition and sort key - The pk will be formed by a unique combination of the partition and sort/range key

  > Each pk attribute must be defined as string, number, or binary. Best practice is to define primary keys that will make sure that the read and write operations will be divided accross several partitions.
---
class: center, middle
# Data reads can be eventually consistent or strongly consistent

---
class: center, middle
# Operations can return up to 1MB of data

---
class: center, middle
# Reads can be performed either as a Query (search by pk) or Scan
---

# Security

- Every operation needs to be performed by an authenticated principal
- Best practice for apps using DynamoDB is to leverage on the EC2 IAM profile to manage credentials
- Support for fine grained policies is provided (table, item or attribute level)

---

# DynamoDB Streams

  > Feature that allows you to keep track of recent changes by accessing a list of item modifications for the last 24-h period. Streams can be enabled via the console, cli, or SDK 
