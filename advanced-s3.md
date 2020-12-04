# S3 MFA-Delete
* generate a code on a separate device to confirm that you want perform some actions.
* In order to use it you need to enable versioning
* Only bucket owner (root account ) can enable/disable MFA-Delete 
* Once you enable it you will need MFA to:
  * permanently delete an object version
  * suspend versioning on the bucket
* It can be only enabled using CLI

# S3 Default encryption
**Default encryption is applied after checking bucket policy.** That means if you have a policy that denies any unencrypted files to be added and you want to add new unencrypted file (to leverage default encryption) it will be rejected because of that policy.

# S3 Access Logs
* You can configure your bucket to logs all (approved/denied) access request.
* Logs will be stored in another bucket.
* You can analyze logs in a data analysis tools or Amazon Athena
* **Never configure logging bucket to be the same as the monitoring bucket!** (log loop)

# S3 Replication
* In order to use it you must enable versioning in source and destination
* CRR - cross region replication
  * Used for: lower latency access, replication across accounts
* SRR - same region replication
  * Used for: log aggregation, replication between prod and test accounts
* Target and source buckets can be in different accounts
* Copying is async
* IAM permission must be given
* After activating only new objects are replicated 
* There is no delete replication (if you delete file the deletion will not be replicated)
* There is no chaining of replication
  * 1 -> 2 
  * 2 -> 3
  * 1 !-> 3

# S3 Pre-signed URL's
* Are used to generate URL for PUT or GET request that will be valid only for specified amount of time.
* Can generate pre-signed URL's using:
  * CLI or SDK for download the file
  * SDK for uploading the file
* Use-cases:
  * Allow only logged-in users to download a video from a S3 bucket
  * Dynamically generate URL for a ever changing user list to download a file
  * Allow temporarily user to upload a file to a precise location in our bucket

# S3 Storage classes
* S3 Standard - General Purposes
  * Use cases: Big Data Analytics, mobile & gaming applications, content distribution
* S3 Standard IA - data less frequently accessed, but requires rapid access when needed
  * Lower cost of storage than S3 Standard
  * Use cases: Data Store for disaster recovery, backups
  * Minimum storage duration: 30 days
* S3 One Zone IA
  * Same as IA but data stored in single AZ
  * Data lost when AZ is destroyed
  * Lower availability (99,5)
  * Lower cost of storage than IA
  * Use cases: secondary backup, data that you can recreate (thumbnails)
  * Minimum storage duration: 30 days
* S3 Intelligent Tiering
  * Auto moves objects between two access tiers based on access patterns
  * Resilient against events that impact entire AZ
  * extra charged for monitoring
  * Minimum storage duration: 30 days
* S3 Glacier
  * Low cost object storage meant for archiving
  * Data retained for longer term (10s of years)
  * Lower cost per storage but extra cost for retreival
  * Objects in glacier are called Archives (max 40TB)
  * Buckets are vaults
  * Retreival options:
    * Expedited: 1 to 5 minutes
    * Standard: 3 to 5 hours
    * Bulk: 5 to 12 hours
    * Minimum storage duration: 90 days
* S3 Glacier Deep Archive
  * Lowest cost for storage
  * Extra cost for retreival
  * Retreival options:
    * Standard: 12 hours
    * Bulk: 48 hours
    * Minimum storage duration: 180 days

# S3 Lifecycle rules
* Can help you manage files (move between storage clasess, remove file etc)
* Transition actions:
  * Move to glacier after 6 months
* Expiration actions:
  * Delete log files after 365 days
  * Can delete old versions (if versioning enabled)
  * Can delete incomplete multipart uploads
* Can be setup for certaing prefix (all files from "folder") or for certain objects tags

# S3 Performance
* You can make at least 3500 PUT/COPY/POST/DELETE request and 5500 GET/HEAD request per second per prefix in a bucket
* It can be impacted by KMS limitation
  * KMS quota per second is 5500, 10000, 30000 based on region
  * You need to ask KMS for key every time you upload/download a file to encrypt/decrypt the file
* You can increase upload by 
  * using Multipart upload
  * using S3 Transfer acceleration using edge locations
  * You can increase download by
    * S3 Byte Range fetches - multipart download
    * Or Retreive only partial of file (head)

# S3 Event Notifications
* We can get notifications when object is created, removed, restored, replicated
* We can apply notification only for some objects (let's say all .jpg files)
* Use case: generate thumbnails of images uploaded to S3
* You can react with SNS, SQS or Lambda Function
* If you want to ensure event notification is sent for every successful write, you should enable versioning on bucket

# AWS Athena
* It is serverless service to perform analytics directly against S3 files
* Uses SQL to query
* Charged per query/data scanned
* Use cases: BI, analytics, reporting, analyze VPC Logs, ELB logs, Cloud Trail trails etc

# S3 Object Lock & GlacierVault Lock
* S3 Object Lock
  * Adopt WORM (Write Once Read Many) model
  * Block an object version from being deleted for a specified amount of time
* GlacierVault Lock
  * Adopt a WORM model
  * Lock the policy for future edits (cannot be changed)
  * Helpful for compliance and data retention