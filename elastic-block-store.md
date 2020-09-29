# EBS Volume (Elastic Block Store)

* It`s just a network drive, so you can quickly detach it and attach it to an instance.
* It`s locked to an AZ, so you can attach it and detach it only for machines in given AZ.
* You pay for what you order (provision), not for real usage. So if you ordered 500GB and use only 1GB, you still pay for 500GB.
* You can increase storage while running

## EBS Volume types

There are 4 types of EBS Volume Types:
* GP 2 (SSD) - **G**eneral **P**urposes. Good price for good performance. Can be used as root volume.
* IO 1 (SSD) - Super speed, low latency, high-performance. Used for most important, critical tasks. (Pricey). Can be used as root volume.
* ST 1 (HDD) - Low-cost HDD, good performance, good for frequently accessed, throughput intensive workloads.
* SC 1 (HDD) - Lowest-cost HDD used for less-frequency workloads.

### GP2
* Recommended for most workloads (cheap)
* Can be used as: Virtual desktops, Low-latency interactive apps, Dev and test environments
* Size range: 1GB - 16TB
* 3 IOPS per GB.
* Min IOPS: 100, Max IOPS: 16 000
* At 5,334TB we are at the max IOPS 

### IO 1
* Recommended for critical business applications (expensive)
* Can be used as Database servers (MongoDB, MS SQL Server, MySQL, Cassandra, Oracle, Postgress DB). Critical databases.
* Size range: 4B - 16TB
* IOPS is provisioned (so-called PIOPS - you select how much IOPS you need, but you pay for it): 100 - 32 000 - 64 000 (min - max - nitro max)
* Maximum ratio of provisioned IOPS is 50:1 per requested GB. So for 4GB you can have up to 200 IOPS.

### ST1
* Recommended for streaming workloads (fast throughput and low price)
* Can be used to: processing Big Data, Datawarehouse, Log processing, Apache Kafka
* Cannot be a boot volume
* Size range: 500 GB - 16 TB
* Max IOPS: 500
* Max throughput: 500 MB/s

### SC1
* Recommended for infrequently accessed data
* Scenarios where the lowest storage cost is important
* Cannot be a boot volume
* Size range: 500 GB - 16 TB
* Max IOPS: 250
* Max throughput: 250 MB/s


## EBS Snapshots
* Backups are incremental (so only changed blocks backups)
* While making a backup IO is using, so you should rather run backup during quiet hours
* Are stored in S3
* Max 100 000 per account
* Can copy between AZ`s or Regions
* Can make AMI from Snapshot
* EBS volumes restored by snapshots need to be pre-warmed
* Snapshot process can be automated using Amazon Data Lifecycle Manager