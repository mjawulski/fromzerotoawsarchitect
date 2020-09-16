
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