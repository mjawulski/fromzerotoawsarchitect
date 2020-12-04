# CLI
* you can create access keys to get access from your personal computer (and you should do it only for personal computer access)
* if you need to access CLI from an EC2 machine you should **ALWAYS** use IAM Roles
* You can specify iam policies in each role
* You can also create your own policy that will grant/deny access to specific resources under given conditions

# AWS EC2 Instance Metadata
* Url: http://169.254.169.254/latest/meta-data/ **slash at the end is very important**
* You can get info about the EC2 instance from that url