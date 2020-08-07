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