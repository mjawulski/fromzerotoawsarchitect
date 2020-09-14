## Scalability
It means that your application can handle increased load by adapting to that load.

It can either scale **verticaly** (add more stuff to the machine like increase CPU, RAM, generally **provide a better machine**) or **horizontally (provide more machines** of the same type and distribute traffic across all of them)

### Vertical scalability
- increase size of the machine
- common for non distributed systems (database)
- can be limited by the available hardware

### Horizontal scalability
- increase number of machines
- common for web servers (www apps)
- **scale out - add more instance)**
- **scale in - remove instances**


## High availability
It means that your application runs in at least 2 different AZs.

We ensure high availability in case any data center goes down.

## Load balancing
Load balancers are the servers to forward traffic to multiple servers (EC2 instance).

They:
- expose single point of access (DNS - nice looking name instead of old, hard to remember IP v4)
- taking care of machines underneath going down by making a regular health checks

### Types of load balancers in AWS

There are three types of Load Balancers in AWS
- Classic load balancer (outdated). Can handle HTTPS, HTTP and TCP traffic
  - Health checks are based on HTTP(s) or TCP
  - Fixed hostname
- Application load balancer. Can handle HTTP, HTTPS and Web Sockets
  - Uses target group to redirect traffic there.
  - Can balance traffic to multiple applications on the same machine
  - Supports WebSockets and HTTP2 and can be used to redirect traffic from HTTP to HTTPs
  - You can route to different target group based on
    - path in URL (mypage.com/**intro** and mypage.com/**about** can be served from different target group)
    - hostname in URL (**michal**.mypage.com vs **wojtek**.mypage.com)
    - query string and headers (mypage.com/intro?**p=1** vs mypage.com/intro?**p=2**)
  - Great for: Microservices, Docker and in general containerized applications
  - So in general it looks like this: USERS ----- ALB (application load balancer) ---- [TARGET GROUP(s)](#target-groups) ---- EC2 Instance(s)
  - ALB can route to multiple [Target groups](#target-groups)
  - Health check made on target group level
  - Instances behind ALB have access to the IP of ALB and can`t see IP of the client directly. If you need to get IP of the client you need to use **X-Forwared-For** header to get IP of the client. (**X-Forwared-Port** for port and **X-Forwared-Proto** for protocol (http/https)) 
- Network Load Balancer. Can handle TCP, TLS and UPD traffic

### <a name="target-groups">Target Groups</a>

They can be one of the following:
- EC2 Instance
- ECS Task
- Lambda Function
- IP Address (private)

