# Amazon SQS - Standard Queue
* Unlimited throughput, you can send unlimited number of messages per second
* Unlimited number of messages in the queue
* By default messages will stay in queue for 4 days (max 14). It is called retention. After that if you won`t process them, they gets deleted.
* Low latency < 10ms on publish and receive
* Max 256KB per message sent
* Can contain duplcated messages
* Can have out of order messages
* Consumer is responsible for delete messages after they have been processed
* Consumer can receive up to 10 messages at a time
* Consumers can leverage Auto Scalling Group to be added/removed based on Cloud Watch Alarm (Queue Length)
  
### Security:
* Encryption:
  * In-flight Encryption using HTTPS API
  * At-rest Encryption using KMS keys
  * Client-side encryption if the client wants to perform encryption/decryption itself
* Access Controls: IAM policies to regulate access to the SQS API
* SQS Access Policies
  * Useful for cross-account access to SQS queues
  * Useful for allowing other services to write to an SQS queue

## SQS - Message visibilty Timeout
* Period of time in which after message is polled by a consumer that message is invisible for other consumers.
* After this time (30s default) if it has not been deleted from the queue it can be polled again by other consumers.
* There is a possibilty for consumer to extend this period by calling ChangeMessageVisibility API 

## SQS - Dead letter Queue
* If consumer fails to process a message within the Visibility Timeout the message goes back to the queue
* We can set a threshold of how many times a message can back to the queue
* After MaximumReceives Threshold is exceeded the message goes into a dead letter queue (DLQ)
* Usefull for debugging why the message has not been processed correctly
* Set high retention to make sure you have time to process it (14 days is good in that case)

## Delay queue
* you can delay (max 15 minutes) delivery of the message for all messages in the queue or for concrete message 

## FIFO queue
* First in first out. Order of the messages will be kept (which is not the case in standard queue)
* Limited throughput (how many messages you can send): 300msg/s or 3000 msg/s if you sent them in batch
* Exactly-once send capabilty (by removing duplicates)
* Messages are processed in order by the consumers
* Name must end with .fifo
* Possibility to remove duplicates based on content

## SNS
* Pub-Sub pattern
* One message consumed by many receivers (subscribers)
* Each subsciber to the topic (subject rxjs :)) will receive all messages (new feature to filter messages)
* Up to 10 000 000 subscriptions per topic
* 100 000 topics limit
* Subscribers can be:
  * SQS
  * HTTPS/HTTPS (with delivery retries)
  * Lambda
  * Emails
  * SMS messages
  * Mobile Notifications
* Subscriptions protocols:
  * HTTP
  * HTTPS
  * Email
  * Email-JSON
  * AMAZON SQS
  * AWS Lambda

### Security:
* Encryption:
  * In-flight Encryption using HTTPS API
  * At-rest Encryption using KMS keys
  * Client-side encryption if the client wants to perform encryption/decryption itself
* Access Controls: IAM policies to regulate access to the SNS API
* SNS Access Policies
  * Useful for cross-account access to SNS topics
  * Useful for allowing other services to write to an SNS topics

## SNS + SQS: Fan Out
* Push once in SNS, receive in all SQS queues that are subscribers
* Fully decoupled, no data loss
* SQS allows for: data persistence, delayed processing and retries of work
* Ability to add more SQS subscribers over time
* Make sure your SQS queue **access policy** allows for SNS to write
* **SNS cannot send messages to SQS FIFO queues (AWS limitation)**

# Kinesis
Big Data. Real Time.
* Managed Alternative to Kafka
* Great for application logs, metrics, IoT, clickstreams (again big data real time)
* Great for streaming processing frameworks (Spark, NiFi...)
* Data is automatically replicated to 3 AZ
* Kinesis has 3 products
  * Kinesis Streams (also called just kinesis) - low latency streaming ingest (process) at scale
  * Kinesis Analytics - perform real-time analytics on streams using SQL
  * Kinesis Firehose - load streams into S3, Redshift, Elasticsearch, DB

## Kinesis Streams 
* Streams are divided in ordered Shards/partitions
* Data stay in stream from 1 (default) to 7 days
* Ability to reprocess / replay data (it is different than SQS where data after processing is gone)
  * After processing data stays in the shard/stream
* Multiple applications can consume same stream (similar to SNS)
* Real-time processing with scale of throughput
* Once data is inserted in Kinesis it can`t be deleted

## Kinesis Streams Shards
* One stream is made of many different shards
* 1MB/s or 1000 messages/s for write per shard
* 2MB/s at read per shard (if you need to have 5MB/s throughput you need 3 shards for read and 5 shards for write)
* You pay for shard, you can have them as much as you want
* You can batch messages
* You can increase/decrease number of shards (reshard/merge)
* Records ordered per shard

## Producers
* While producing data you need to provide Message Key (string that will be hashed)
* Same keys goes to the same shards
* Choose a partition key that is highly distributed (prevents "hot partition" where most of the data is stored on the same partition)
  * user id is good
  * country name is not good when you have many users from the same country
* Use batching to reduce costs and increase throuput
* ProvisionedThroughputExceeded exception when exceeding limits. You can use retry and exponential backoff in that case, or increase shards or ensure paritition key is good and there is no hot partitions.
* Can use CLI, AWS SDK or producer libraries from many frameworks

## Consumers
* Can use a normal consumer (CLI, SDK, etc)
* Or you can use KCL to consume data efficently
  * Kinesis Client Library (in Java, Node, Python, Ruby and .NET)

## Security
* Control acces/authorization using IAM Policies
* Encryption in flight using HTTPS
* Encryption at rest using KMS
* Possibility to encrypt/decrypt data client side
* VPC Endpoints available for Kinesis to access withing VPC

# Kinesis Data Firehose
* Fully managed service to load data into Redshift/Amazon S3/ Elastic Search and Splunk
* Near Real Time
  * 60s latency minimum for non full batches
  * Or minimum 32 MB of data at time
* Supports many data formats, conversions, transformations, compression
* Pay for amount of data groing through Firehose
* No data storage in Firehose directly

# Kinesis Data Analytics
* Perform real-time analytics on Kinesis Streams using SQL
* Auto scaling
* Managed
* Real-time
* Pay for actual consumption rate
* Can create streams out of the real-time queries

# SQS vs SNS vs Kinesis
* SQS
  * Consumer pulls data
  * Data is deleted after being consumed
  * Can have as many consumers as we want
  * No need to provision throughput
  * No ordering guarantee (except FIFO queues but then limited throughput)
  * Individual message delay capability
* SNS
  * Push data to many subscribers (pub/sub)
  * Up to 10 000 000 subscribers
  * Data is not persisted (lost if not delivered)
  * No need to provision throughput
  * Integrates with SQS for fan-out arch. pattern
  * 100 000 topics limit
* Kinesis
  * Consumers pulls data
  * As many consumers as we want (one consumer per shard)
  * Possibility to replay data
  * Real time, analytics big data ETL 
  * Ordering at shard level
  * Data retention
  * Must provision throughput

# Amazon MQ
* Traditional on-prem apps may use open protocols such as MQTT, AMQP, STOMP...
* When migrating to the cloud instead of re-engineering the application to use SQS and SNS we can use Amazon MQ
* Amazon MQ = Apache Active MQ
* Amazon MQ doesn`t scale as much as SNS/SQS
* Amazon MQ runs on a dedicated machine, can have multiple AZ with failover
* Amazon MQ has both queue feature SQS and SNS