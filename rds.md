# Relational database service (RDS)

It`s a managed database service with SQL as a query language.
It let`s you create a database that is managed by AWS
DB Engines suported:
* Postgres
* MySQL
* MariaDB
* Oracle
* Microsoft SQL Server
* Aurora

## RDS Backups
* Automatically enabled
* Are Automated
* You get daily full backup of the db
* Transaction logs are backed-up every 5 minutes (so you can restore to any point in time up to 5 minutes ago)
* are kept 7 days by default but it can be increased to 35 days
  
## DB Snapshots
* IT`s a backup manually triggered by the user
* are kept as long as you want

## RDS Read Replicas
* You can have up to 5 read replicas. It helps you when you have a lot of reads in your app (dashboard)
* They can be within AZ, Cross AZ or Cross Region
* Replication is ASYNC - so there can be some cases where data is not consistent (that`s ok)
* Replicas can be promoted to be an independent DB (reads => reads and writes)
* Apps must update Connection String to use Read Replicas
* You can`t insert, update or delete data. **Only READS**
* Use cases:
  * Reporting applications
  * Run analytics
  * Dashboards

### Network cost
* If you transfer data between different AZ`s it will cost you money
* But if you setup Read Replicas within same AZ there is no network cost

## RDS Multi AZ (Disaster Recovery)
* You can setup a standby DB to be used as Disaster Recovery
* Data is replicated SYNC so every writes are replicated in same time
* In case of loss of AZ, loss of network, instance or storage failure - stanby DB is going to be the main one
* From client perspective you get one DNS name - so failover is automatic (you don`t know what is the DB you are talking to)
* Not for scaling it`s just standby db that is not in use for most of the time

## RDS Security
* At Rest encryption:
  * Possibility to encrypt the master & read replicas with AWS KMS
  * Has to be defined at launch time
  * **If the master is not encrypted then Read Replicas cannot be encrypted**
  * Transparent Data Encryption only for Oracle and SQL Server
* In flight Encryption
  * SSL while connecting to DB
* Snapshots of un-encrypted db is also un-encrypted
* Snapshots of encrypted db is encrypted
* Can copy an un-encrypted snapshot into encrypted one
* IAM policies can control who can manage AWS RDS (create db, remove it etc, not who can access it). Username and password can be set up to connect to db.
* IAM-based auth can be used to login into RDS MySQL & PostgreSQL

# Aurora
* Aurora storage automatically grows from 10GB to 64TB
* Shared storage volume
* is cloud optimized (5x better performance than MySQL on RDS and 3x better than Postgres)
* can have 15 replicas and replication process is faster
* it`s high available by design, failover is instantaneous
* cost about 20% more than RDS
* creates 6 copies of data across 3 AZ
* 4/6 needed for writes
* 3/6 needed for reads
* Self-healing
* Auto scaling and replication
* Instant failover
* One master that takes writes, multiple read replicas
* Support for cross-region replication
* Exposes writer endpoint for applications they use it to make writes to db. Because master can change.
* Exposes Reader Endpoint for applications they use it to read from db. Because there can be many Read Replicas
* Security the same as for other RDS

## Aurora serverless
* Automated database instantiation and auto-scaling based on actual usage
* Good for infrequent, intermittent or unpredictable workloads
* No capacity planning needed
* Pay per second

## Global Aurora
* 1 primary region that takes reads and writes
* Up to 5 secondary regions (read-only). Replication lag is around 1s
* Up to 16 Read Replicas per secondary region
* Helps for decreasing latency
* Promoting another region in less than 1 minute

# ElastiCache
* Managed Redis or Memcached (in-memory database with high performance, low latency)
* Helps to make app stateless
* Write scaling using sharding/Read scaling using read replicas
* Multi AZ with failover capability
* It is used to relieve the load off from the database and to keep some data (ie user session)

## Redis
* Similar to RDS (ReDiS see similarities? :)
* Think:
  * 2 instances (primary, replica)
  * data persistance
  * backup/restore
* MultiAZ
* Read replicas
* Data durability

## Memcached
* Multinode for sharding (data partitioning)
* Non-persistent
* No backup no restore

## Elasticache Security
* All (redis and memcached) support SSL in flight encryption
* **Do not support IAM auth** 
* IAM is only used to AWS API lvl security (to manage)

### Redis AUTH
* You can set password/token to be used with your queries
* You can set up security group

### Memcached
* supports SASL-based auth