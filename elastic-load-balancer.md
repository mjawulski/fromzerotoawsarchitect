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
- **scale out - add more instance**
- **scale in - remove instances**


### High availability
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
  - Can handle milions of requests per second
  - Faster, more perfomant than ALB, lower latency (100ms vs 400ms)
  - Has one static IP per AZ and supports Elastic IP (**note: CLB and ALB has static hostname, not static IP like NLB**)
  - If you need extreme perfomance or need to handle TCP, TLS or UDP traffix then you need to use Network Load Balancer (NLB)

### Stickiness
If you want one client to be always redirected to the same EC2 instance you need to use Stickiness.
works for **CLB and ALB only**. You can use it to keep session data for the client on the server.

### Cross zone load balancing
In CLB is disabled by default. No charge if you enable it.

In ALB it is always on. No charges.

In NLB it is disabled by default. If you enable then you pay for it.

### SSL Certs
- You can encrypt your traffic and use HTTPS. 
- To do so you can use ACM (AWS Certificate Manager) to create certificate 
  - or create and upload your own certificate
- SNI (Server name indication)
  - If you have multiple websites on one server and they all using certificates then you need to find a way to serve correct certificate. This way is called SNI. It`s a new protocol that returns Certificate based on client request (client sends hostname to connect to). It can also returns a default one. Works only with ALB and NLB **not for CLB**.  

### Conection Draining (a.k.a. Deregistration delay)
It`s a time to complete existing request while the instance is being deregistered or unhealthy.
In other words you can set up some time to let request finished before machine is turned off.

## Auto Scaling Groups (ASG)
- It allows you to automatically scale in (remove instance) or scale out (add instance) to match current load.
- you can also define a minimum and maximum number of machines running
- **the whole purpose of having ASG is that if an instance get terminated for whatever reason ASG will recreate it automatically. Which brings us more safety**
  - They can be terminated if for example Load Balancer mark them as unhealthy (and to make them healty again is sometimes good to terminate it and create a new one)

You need to define three parameters:
- minimum size: minimum number of instances that will run for sure
- actual size/desired capacity: number of instances that are currently running
- maximum size: maximum number of instances that can be run

### Auto Scaling Alarms
- You can set up policies that will scale up/scale down based on alarms
- alarms can be actually anything (average cpu, network response time) - they are calculated as average for all instance
  - you can even set up custom alarms like number of users using applications etc
  - you can set up them based on schedule (I know that on black monday or every tuesday more users come in)

### Scaling policies
- Target tracking Scaling
  - I want average of CPU to stay around some value
- Simple/Step scaling
  - If alarm is triggered then add or remove some number of instances
- Scheduled actions
  - Add/Remove some numbers of instances every tuesday at 10AM

Scaling cooldown is a period of time after scaling up/down when there can be no additional scaling up/down. It is to helps previous scaling takes effect. You can use it to optimize cost after removing some instances. You can set up a scaling cooldown to concrete policy that overides default cooldown for ASG and let`s say terminate some instances faster.

## Tips for Solution Architects
What is ASG termination policy?
1. Choose AZ which has the most number of instances.
2. Delete that instance which has the oldest configuration

You can leverage lifecycle hooks when ASG creates/terminates new EC2 instance.

You can use it to send logs to some place before instance is being terminated or to do some additonal checks before instance starts.

### <a name="target-groups">Target Groups</a>

They can be one of the following:
- EC2 Instance
- ECS Task
- Lambda Function
- IP Address (private)

