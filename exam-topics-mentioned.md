
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