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
* you can delay delivery of the message for all messages in the queue or for concrete message 

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

### Security:
* Encryption:
  * In-flight Encryption using HTTPS API
  * At-rest Encryption using KMS keys
  * Client-side encryption if the client wants to perform encryption/decryption itself
* Access Controls: IAM policies to regulate access to the SNS API
* SNS Access Policies
  * Useful for cross-account access to SNS topics
  * Useful for allowing other services to write to an SNS topics