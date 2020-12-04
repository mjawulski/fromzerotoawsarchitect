# Storage Extras

# Snowball
## Snowball
* 80 TB storage
* Physical data transport that helps moving data in/out AWS
* Alternative to move data over the network and pay network fee
* Secure, tamper resistant, KMS 256 encryption
* Pay per data transfer job
* Use cases: large data cloud migration, Data center decommision, disaster recovery
* If it takes more than a week to transfer over network use Snowball device

## Snowball Edge
* Same as Snowball but let's you do some computation while data is moving, directly on the device
* 100/50 TB storage with 24/52 vCPU added
* Supports custom EC2 AMI
* Supports custom Lambda functions
* Useful to pre-process data while moving
* Use case: data migration, image collation, IoT capture, machine learning

## Snowmobile
* Transfer exabytes of data 
* Each Snowmobile has 100 PB of capacity
* If you transfer more than 10PB is better than snowball

# Storage Gateway
* Part of the infrastructure is on the cloud
* Part of the infrastructure is on-premise
* Storage gateway is a way to expose S3 data to be accessible on-premise
* Bridge between on-premise data and cloud data in S3
* Use cases: disaster recovery, backup & restore, tiered storage
* There are 3 types of storage gateway
  * File gateway
  * Volume gateway
  * Tape gateway

## File Gateway
* S3 buckets can be accessible using NFS and SMB protocol
* Supports all kinds od S3 buckets (standard, IA, Glacier)
* Provides access through IAM roles for each File Gateway
* Most recently data is cached in the file gateway
* Can be mounted on many servers

| Private Data Center                    |      |     AWS     |
Application Server(s) <----> File Gateway <---------> S3 Bucket

## Volume Gateway
* Block storage using iSCSI protocol backed by S3
* Backed by EBS snapshots which can help restore on-premise volumes
* Cached volumes: low latency access to most recent data
* Stored volumes: entire dataset is on-premise, scheduled backups to S3

| Private Data Center                           |          |     AWS                        |
Application Server(s) <--iSCSI--> Volume Gateway <---------> Storage Bucket in S3---Amazon EBS snapshots

## Tape Gateway
* Some companies are still using physical tapes
* Backup data using existing tape-based processes (and iSCSI interface)
* Works with leading backup software vendors

| Private Data Center                |           |     AWS     |
Backup server <--iSCSI--> Tape Gateway <---------> Virtual Tapes stored in S3 --- Archived Tapes stored in Glacier

## Hardware appliance
* If you have small data center and don`t need/have virtualization you can buy hardware appliance to leverage Storage Gateway

# Amazon FSx
## Amazon FSx for Windows
* Shared File System that works for Windows
* Supports SMB protocol & Windows NTFS
* Integrated with MS Active Directory
* Built on SSD scale up to 10s of GB/s, millions of IOPS
* Can be accessed from on-prem infrastructure
* Can be Multi-AZ
* Data is backedup daily to S3

## Amazon FSx for Lustre (linux clustre)
* built for large-scale computing
* machine learning, High Performance Computing (HPC)
* Video processing, financial modeling, electronic design automation
* Scales up to 100s of GB/s, millions of IOPS, sub-ms latencies
* Seamless integration with S3
  * You can read S3 as a file system
  * You can write output of computations to S3
* Can be accessed from on-prem infrastructure

# Storage Options Comparision
* S3: Object storage, don`t need to provision size ahead of time
* Glacier: Object archival, retreive it rarely and we can wait for retreival
* EFS: Network File System for Linux, accesible for all EC2 instances at once, can be multi AZ
* FSx for Windows: Network file sisytem for Windows
* FSx for Lustre: High Performance Computing (HPC) Linux file system
* EBS: Network storage for one EC2 instance at a time. Bound to specific AZ. Can be moved to another AZ through snapshots.
* Instance storage: Physical storage for EC2. High IOPS. When you loose EC2 you loose Storage as well.
* Storage gateway: File, Volume, Type to access AWS data from on-prem infrastructure. Hybrid cloud.
* Database: for specific workloads, usually with indexing and querying.