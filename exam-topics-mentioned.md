
## EC2 for Solution Architects
- EC2 instance are billed by the second. t2.micro is free tier.
- Strategies to allocate spot instances (lowest price)
- If you have timeout issues please check if you have configured security group correctly
- If you experience any permission issues on the SSH key then you need to run *chmod 0400*
- Security groups can reference other security group (**security groups are popular exam questions so learn it properly**)
- you should know differences between private, public and elastic IP
- you can customize EC2 instance at boot time using EC2 User Data
## Load Balancers  
- If you need extreme perfomance or need to handle TCP or UDP traffix then you need to use Network Load Balancer (NLB)
- If you hear multiple certificates think ALB or NLB
- When you see Launch template vs Launch Configuration you should pick Launch template which is newer version
## EBS & EFS
- EBS Encryption leverages keys from KMS (AES-256)
- On the exam when you are asked EBS vs Instance store ask yourself Am I okay losing the data (if you are making cache, temp data, buffer then you`re good to choose Instance store)
- Instance store can have very much IOPS and EBS are limited to 64k IOPS so if there is a question with hundreds or thousands of IOPS choose Instance store
- EBS volumes can be used in RAID0 to increase performance and RAID1 to increase fault tolerance
- EFS Works only with Linux (not with Windows)
## RDS
- Read Replicas can be setup as Multi AZ for Disaster Recovery
- **If the master is not encrypted then Read Replicas cannot be encrypted**
- if you hear "infrequent, intermittent or unpredictable workloads" think aurora serverless
- Redis vs Memcached - if you need AZ, Read Replicas, Data durability, Backup, Restore then choose Redis. If you don`t need AZ, Read Replicas, backup, restore, and you need sharding choose Memcached
## S3
- any time you hear access to a specific resource for certain users for limited amount of time - think pre-signed URL's
- any time you see filtering of data server-side to get less data think about S3 Select or Glacier Select
- any time they ask about analyze data directly on S3 -> use Athena
- if you hear we want to keep that file for 7 years and prevent it from being deleted think GlacierVault Lock
- if you want to import data from Snowball to Glacier you have to import it to Amazon S3 first and then use S3 lifecycle policy
- you need to know exact differences between all three storage gateways types

## Storage Gateways
- if you hear iSCSI protocol that might be tip to use Volume Gateway (not always, Tape Gateway share also iSCSI protocol)
- if you hear anything around backup and virtual tapes that might be tip to use Tape Gateway
- Question will hint at which gateway to use. Read it well.
- "...on-premise data to the cloud..." => Storage Gateway
- File access / NFS => File Gateway
- Volumes / Block Storage / iSCSI => Volume Gateway
- VTL Tape Solution / Backup with iSCSI => Tape Gateway
- NFS Backup in small data center (with no virtualization) think Hardware appliance

## Decoupling
- anytime you hear decoupling think about Amazon SQS
- anytime you hear SNS sends messages to SQS Fifo you can rule out that answer - this is not possible
- anytime you hear big data, realtime, IOT, ETL you should pick Kinesis
- anytime you hear we are migrating an app from on-prem to cloud and the app is using MQTT or AMQP what should we use? Answer: Amazon MQ