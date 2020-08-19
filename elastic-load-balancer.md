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
- expose single point of access (DNS - nice looking name instead of old, hard to remmember ip v4)
- taking care of machines underneath going down by making a regular healtchecks

### Types of load balancers in AWS

There are three types of Load Balancers in AWS
- Classic load balancer (outdated). Can handle HTTPS, HTTP and TCP traffic
- Application load balancer. Can handle HTTP, HTTPS and Web Sockets
- Network Load Balancer. Can handle TCP, TLS and UPD traffic
- **TO BE COMPLETED**
