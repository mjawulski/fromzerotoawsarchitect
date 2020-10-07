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

## EBS Encryption
* You can enable encryption in EBS and it will get you:
  * Encryption of all the data
  * Snapshot created will also be encrypted
  * You don`t have to do anything (transparency)
* Encryption has minimal impact on latency
* EBS Encryption leverages keys from KMS (AES-256)
* Copying unencrypted snapshots allows encryption
* Snapshots of encrypted volumes are encrypted

## Instance store
* Some instances instead of EBS attached can have something called **Instance Store**
* It is a drive physically attached to the machine (not a network drive like EBS)
* Pros:
  * Better IO performance
  * Good for buffer/cache/temp content/scratch data
  * Data survives reboots
* Cons:
  * On stop or termination data is lost
  * **You can`t resize instance store**
  * Backups are handled by the user not the AWS
  * On the exam when you are asked EBS vs Instance store ask yourself Am I okay losing the data
* Instance store can have very much IOPS and EBS are limited to 64k IOPS so if there is a question with hundreds or thousands of IOPS choose Instance store

## EBS RAID
* RAID 0 (Increase performance) - we can have a very big disk with a lot of IOPS
  * Combining 2 or more volumes and getting the total disk space and I/O
  * If one disk fail all the data is failed
  * Use cases:
    * Apps that needs a lot of IOPS and doesn`t need fault tolerance
    * A database that has replication already built in
  * Individual properties will sum up: 2 disk each with 100GB and 300IOPS will get us 200GB and 600 IOPS in total
* RAID 1 (Increase fault tolerance) - all the data is replicated through all the disks (mirroring volume to another)
  * If one disk fails the other is still working
  * Use cases:
    * Application that need increase volume fault tolerance
    * Application where you need to service disks
    * Individual properties won`t sum up: 2 disk each with 100GB and 300IOPS will get us 100GB and 300 IOPS in total

## EFS (Elastic file system)
* Managed network file system (NFS) that can be mounted on many EC2
* EFS works with EC2 instances in multi AZ
* Highly available, scalable, expensive, pay per use
* Use cases: content management, web serving, data sharing, wordpress
* Works only with Linux (not with Windows)
* Performance and storage clases:
  * EFS Scale: can handle 1000s of concurent EC2 clients, 10GB+ throughput, can grow to petabytes automatically
  * Performance mode: 
    * General Purpose (default): latency-sensitive use cases (web server, CMS, etc)
    * MAX IO: higher latency, higher throughput, highly paralel (big data, media processing)
  * Storage tiers:
    * Standard: for frequently accessed files
    * Infrequent access (EFS-IA): pay for retreive, pay less for storage