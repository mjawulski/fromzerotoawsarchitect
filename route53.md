# Route 53
* Route 53 is a managed DNS (Domain Name System)
* DNS is a collection of rules and records that helps clients understand how to reach a server. For example browser has a name of a website and wants to know how to reach a server where this site is deployed.
* In AWS the most common records are:
  * A: hostname to IPv4
  * AAAA: hostname to IPv6
  * CNAME: hostname to hostname
  * Alias: hostname to AWS Resource
* It can use
  * public domain name you own
  * private domain names that can be resolved by your instances in your VPC

## TTL
* Time to live - how long clients (browsers) will cache the response for DNS query

## CNAME vs Alias
* CNAME points non-root hostname to another hostname.
* ALIAS points root and non-root hostname to an AWS resource (anything like LoadBalancers etc with .amazonaws.com)
* Native healthcheck
* CNAME is paid, ALIAS is free

## Routing policies
* Simple routing policy
  * Can`t attach health checks
  * If multiple values returned, client will choose random one
  * Use when redirect to a single resource
* Weighted Routing policy
  * Control % of the request that goes to specific endpoint
  * If you want to test new version of app available to the 5% of the users that`s the way to go
  * Helpful to split traffic between two regions
  * Can attach healthchecks
* Latency Routing policy
  * Redirect to the server that has the least latency close to us
  * Helpful when latency of users is priority
  * Germany may be redirected to US if that`s the lowest latency
* Failover Routing policy
  * If you enable heath checks you are able to use failover routing policy
  * It will return failover address in case primary is unhealthy
* Geo Location Routing policy
  * Based on user location
  * Traffic from given country should go to a specific IP
  * Need to add default policy in case there is no match on location
* Multivalue Routing Policy
  * Use when routing trafic to multiple resources
  * Up to 8 healthy records are returned for each Multivalue query
  * It is not substitute for ELB

### Health checks
* Can have HTTP, TCP and HTTPS health checks (no SSL verification)
* Possibility to integrate with CloudWatch
* Can be linked to Route53 DNS queries and change them