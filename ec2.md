## Elastic IP
Every Ec2 instance gets public IP on start. But it changes when you restart machine. So if you need a **static public IP** then you are going to need **Elastic IP**.
It is IP v4. And it is yours until you delete it. You can map any EC2 instance to it so it can be used in such case when you lost one of your instance you can quickly change target Elastic IP machine to another working instance. But you should avoid it and use Load Balancer instead.

## EC2 Instance Launch types

- **On-demand instance** - you want to use it for a short amount of time. You pay as you go (per second, after the first minute).  
  - Pros: 
    - predictable pricing
    - no long term commitment 
  - Cons: 
    - The Highest cost per minute
  - Recommended usage: short term workload where stability matters (go for: additional resources in peaks, **elastic** needs, no-go: databases)
- **Reserved** -  you know ahead of time that you are going to need an ec2 machine for a minimum of 1 or 3 years. You have to pay upfront for it.
  - Reserved instance - regular ec2 machine that is going to run **at least one or three years** (size of this machine is determined at ordering and cannot be changed). So let`s say it is XL ec2 machine running for at least 1 year.
    - Pros:
      -  cost-effective solution
    - Cons:
      - Type of an instance cannot be changed after ordering
    - Recommended usage: database
  - Convertible Reserved Instance - same as the reserved instance but **you can change the instance type anytime**
  - Scheduled Reserved Instances - for at least a year I am going to need XL machine **every Monday between** 2 PM - 3 PM
- **Spot Instance** - you are going to need this machine for a short period, and you don`t want to pay too much but you agree that **you can lose instances**. You will loose an instance if a maximum price you are willing to pay for the spot instance is lower than the current spot price
  - Pros
    - Discount up to 90% (most cost-effective)
    - Ideal for a short workload that **can be** interrupted/restarted (batch processing, image processing, data analysis)
  - Cons
    - Can be stopped anytime
- **Good use case**: web application using N amount of reserved instances and for peaks additional spot instance or on-demand instance (if you can/cannot break the work)
- **Dedicated Instance** - only you are using the hardware underneath, but it can be shared with other instances within the same account.
- **Dedicated hosts** - entire dedicated physical server (most powerful, and pricey) for an instance. You can order it **only for 3 year period** . You have visibility into the sockets/cores used by a host. Full control of ec2 install placement. Useful if you are running software that bills you based on a number of cores your machine is using.
  - Pros:
    - full control
    - you can use software with a complicated license on this type of ec2 (strong compliance needs like - you cannot share the hardware if you want to work with us)
  - Cons:
    - most pricey (the worst cost-effective solution)

## Spot Instance 
You define a max price you are willing to pay. AWS is using its spare resources and defines a **spot price** (less resources they have, bigger spot price. It varies from AZ to AZ). If the spot price is greater than your maximum spot price then your EC2 instance is reclaimed. In that situation you have 2 minutes to do either of the following:
* stop the instance. If you want to continue sometime in the future.
* terminate the instance. If you don`t care about the state and want to get a fresh ec2 instance once the current spot price go below your maximum price again.

You can also create a **spot block** if you don`t want your ec2 instance to be reclaimed. You can  book ec2 instance from 1 to 6 hours and in that case your instance would not be reclaimed (but according to documentation in may happen in a rare situation).

**Spot instances are great for batch jobs, data analysis and in general any type of work that is resilient to failure and should not be used in opposite cases like critical jobs or used as a database server**

### Spot request
When you want to create a spot instance you have to create a spot request where you define things like maximum spot price, the number of instances, launch specification (AMI), valid from and valid to etc.

One of the properties of a spot request is **request type**. You can create **one-time** spot request or a **persistent** spot request.
One-time spot request means that after satisfying conditions in the spot request your instances got created and later your instances gets terminated/stopped (because current spot price > maximum spot price), they will never get started again.
Persistent spot request means instances will get created every time all conditions are satisfied (max spot price>current spot price, valid from < current date < valid to, number of currently running instances < desired number of instances). This has following consequences: If you want to stop/terminate an instance and you have persistent spot request, you must first cancel spot request *before* stopping instance. Otherwise instances will get relaunched.

One important thing about canceling spot request. Canceling spot request **will not** terminate the instances (you have to do it on your own, or wait until current spot price > maximum price)

## spot fleets
The Ultimate way to save money.

Spot fleet is a set of spot instances and optional on-demand instances.

The Spot Fleet attempts to launch the number of Spot Instances and On-Demand Instances to meet the target capacity that you specified in the Spot Fleet request. The request for Spot Instances is fulfilled if there is available capacity and the maximum price you specified in the request exceeds the current Spot price. The Spot Fleet also attempts to maintain its target capacity fleet if your Spot Instances are interrupted.

You can also set a maximum amount per hour that you’re willing to pay for your fleet, and Spot Fleet launches instances until it reaches the maximum amount. When the maximum amount you're willing to pay is reached, the fleet stops launching instances even if it hasn’t met the target capacity.

A Spot Instance pool is a set of unused EC2 instances with the same instance type (for example, m5.large), operating system, Availability Zone, and network platform. When you make a Spot Fleet request, you can include multiple launch specifications, that vary by instance type, AMI, Availability Zone, or subnet. The Spot Fleet selects the Spot Instance pools that are used to fulfill the request, based on the launch specifications included in your Spot Fleet request, and the configuration of the Spot Fleet request. The Spot Instances come from the selected pools. 

AWS will try to launch instances that satisfy your demands based on one of the following strategies:
 - lowestPrice
The Spot Instances come from the pool with the lowest price. This is the default strategy.

- diversified
The Spot Instances are distributed across all pools.

- capacityOptimized
The Spot Instances come from the pool with optimal capacity for the number of instances that are launching.

- InstancePoolsToUseCount
The Spot Instances are distributed across the number of Spot pools that you specify. This parameter is valid only when used in combination with lowestPrice.

### Choosing an appropriate allocation strategy
You can optimize your Spot Fleets based on your use case.

If your fleet is small or runs for a short time, the probability that your Spot Instances may be interrupted is low, even with all the instances in a single Spot Instance pool. Therefore, the lowestPrice strategy is likely to meet your needs while providing the lowest cost.

If your fleet is large or runs for a long time, you can improve the availability of your fleet by distributing the Spot Instances across multiple pools. For example, if your Spot Fleet request specifies 10 pools and a target capacity of 100 instances, the fleet launches 10 Spot Instances in each pool. If the Spot price for one pool exceeds your maximum price for this pool, only 10% of your fleet is affected. Using this strategy also makes your fleet less sensitive to increases in the Spot price in any one pool over time.

With the diversified strategy, the Spot Fleet does not launch Spot Instances into any pools with a Spot price that is equal to or higher than the On-Demand price.

To create a cheap and diversified fleet, use the lowestPrice strategy in combination with InstancePoolsToUseCount. You can use a low or high number of Spot pools across which to allocate your Spot Instances. For example, if you run batch processing, we recommend specifying a low number of Spot pools (for example, InstancePoolsToUseCount=2) to ensure that your queue always has compute capacity while maximizing savings. If you run a web service, we recommend specifying a high number of Spot pools (for example, InstancePoolsToUseCount=10) to minimize the impact if a Spot Instance pool becomes temporarily unavailable.

If your fleet runs workloads that may have a higher cost of interruption associated with restarting work and checkpointing, then use the capacityOptimized strategy. This strategy offers the possibility of fewer interruptions, which can lower the overall cost of your workload.

## EC2 Instance types
- R: Applications that needs a lot of RAM - in-memory db, in-memory cache
- C: Applications that needs a good CPU - compute/databases
- M: applications that are balanced (think: "medium") - general purposes/web app
- I: Applications that need good local I/O - storage, databases
- G: applications that needs a good GPU - video rendering/machine learning
- T2/T3 - burstable instances

## placement groups
How you want to place EC2 instances compared one to another.
You can define it by using placement group.
When you create a placement group you specify one of the following strategies:
- Cluster - cluster instances into a low latency group in a single AZ (good for high performance computing, bad for availability - AZ is down all instances are in trouble)
- Spread - spread instances across underlying racks (hardware) limit: max 7 per AZ - good for critical apps and high availability
- Partition spread instances across many different partitions within AZ. Max 100 instances per group (good for Hadoop, Kafka, Cassandra)

## elastic network interfaces (ENI)
- it gives EC2 access to the network
- bound to specific AZ
- you can create ENI independently and then attach it on the fly to any ec2 instance in case of failover
- It can have one primary private IPv4, and many secondary IPv4, elastic IP, public ip and security groups.