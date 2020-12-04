# AWS Cloud Front
* It is Content Delivery Network (CDN)
* Improves read performance, content is cached at the edge
* DDoS protection, integration with shield, AWS Web Application Firewall
* Can expose HTTPS and talk to internal HTTPS backends

## What can use Cloud Front. What is Origin
* Origin is a service (S3, ALB, EC2 etc) that can talk to and leverage Cloud Front
* The main origins are
  * S3 bucket
    * Distributing files and caching them at the edge
    * Enhanced security with CloudFront Origin Access Identity (OAI) - you can configure S3 bucket in that way that only Cloud Front has access to it
    * Cloud Front can be used as an ingress (to upload files to S3)
  * Custom Origin
    * Application Load Balancer
    * EC2 instance
    * S3 Website (you must configure bucket as a static S3 website)
    * Any HTTP(s) backend you want

## Cloud Front Security
* If you are using S3 bucket you can configure IAM role to grant access for Cloud Front to access S3 bucket. S3 Bucket can stay private.
* If you are using EC2 Instance. They have to be public. But you can configure security groups to allow public access only from IP's of Cloud Front Edge Locations.
* If you are using ALB They have to be public. But you can configure security groups to allow public access only from IP's of Cloud Front Edge Locations. And EC2 instance can stay private.

## Cloud Front Geo Restrictions
* You can create blacklist and whitelist of who can't/can access your distribution
* Country is determined using 3rd party database
* Use case: Copyright Laws to control access to content

## Cloud Front vs Cross Region Replication
* Cloud Front
  * Configured to be globally out of the box
  * Files are cached (so if you update then it might take a while before users can see the change)
  * Greate for static content that must be available everywhere
* S3 Cross Region Replication
  * You must setup for each region you want replication to happen
  * Files are updated near real-time
  * Read only
  * Great for dynamic content that needs to be available at low-latency in few regions

## Cloud Front Signed URL/Signed Cookie
* Same use case - distribute shared content to premium users all over the world
* You can attach a policy with
  * URL Expiration
  * IP ranges that can access data 
  * Trusted signers (which AWS accounts can create signed URL's)
* How long should the URL be valid for?
  * Shared content (movie/music): short (few minutes)
  * Private content (private to the user): long (days, years)
* Signed URL - access to individual files
* Signed Cookies - access to multiple files

### Cloud Front Signed URL vs S3 Pre-signed URL
* Note the difference in the name (signed vs pre-signed)
* Cloud Front Signed URL
  * Allow access to a path no matter the origin (S3, EC2, ALB)
  * Account wide key-pair, only root can manage it
  * Can filter by IP, path date and expiration
  * Can leverage caching features
* S3 pre-signed URL
  * Allow access to S3 buckets only
  * Uses IAM key of the signing IAM principal
  * Limited lifetime

## AWS Global Accelerator
* Works with **Elastic IP, IC2 Instances, ALB, NLB, public or private**
* Consistent performance
  * Intelligent routing to lowest latency and fast regional failover
  * No issue with client cache (IP doesn't change)
  * Internal AWS network
* Health Checks
  * Global Accelerator performs a health check of your applications
  * Failover less than 1 minute for unhealthy
  * Great for disaster recovery
* Security
  * Only 2 external IP needs to be whitelisted
  * DDoS protection thanks to AWS Shield
