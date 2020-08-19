# Security groups

They controls inbound and outbound traffic between ec2 machine and the Internet/network. You can think of it as a kind of a firewall.

WWW --- **SECURITY GROUP** --- EC2 machine

- Can be attached to many instances.
- One instance can have many Security Groups.
- Locked down to a region
- If any traffic is blocked by the Security Group, ec2 instance will not even see it.
- That means if you have a timeout issues you probably have a problem with Security groups (wrong configuration).
- But if you get a connection refues then SG works fine and probably it is a problem with an APP rather than SG
- all **inboud traffic** is **blocked** by default
- all **outbound traffic** is **allowed** by default

## Security group can reference each other
What is good in secruity groups is that they can reference each other. So in case configuring inbound rules (on machine A) you can just type Security group name instead of IP. Then you attach this security group to a given machine (B) and without any other steps you can acces machine A from Machine B.