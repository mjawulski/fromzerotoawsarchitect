## EBS Volume (Elastic Block Store)

* It`s just a network drive, so you can quickly detach it and attach it to an instance.
* It`s locked to an AZ, so you can attach it and detach it only for machines in given AZ.
* You pay for what you order (provision), not for real usage. So if you ordered 500GB and use only 1GB, you still pay for 500GB.
* You can increase storage while running

THere are 4 types of EBS Volume Types:
* GP 2 (SSD) - **G**eneral **P**urposes. Good price for good performance. Can be used as root volume.
* IO 1 (SSD) - Super speed, low latency, high-performance. Used for most important, critical tasks. (Pricey). Can be used as root volume.
* ST 1 (HDD) - Low-cost HDD, good performance, good for frequently accessed, throughput intensive workloads.
* SC 1 (HDD) - Lowest-cost HDD used for less-frequency workloads.

### GP2
* Recommended for most workloads
* Can be used as: Virtual desktops, Low-latency interactive apps, Dev and test environments
* Size range: 1GB - 16TB
* 3 IOPS per GB.
* Max IOPS: 16 000
* At 5,334TB we are at the max IOPS 

### IO 1
* Recommended for critical bussines applications
* Can be used as Database servers (MongoDB, MS SQL Server, MySQL, Cassandra, Oracle, Postgress DB). Critical databases.
* Size range: 4B - 16TB
* IOPS is provisioned (so called PIOPS - you select how much IOPS you need, but you pay for it): 100 - 32 000 - 64 000 (min - max - nitro max)
* Maximum ratio of provisioned IOPS is 50:1 per requested GB. So for 4GB you can have up to 200 IOPS.