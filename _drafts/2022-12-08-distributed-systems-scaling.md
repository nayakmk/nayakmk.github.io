---
layout: post
title: 'Distributed Systems: Scaling'
date: '2022-12-08 09:48:39 +0530'
---

### Scalability Basic Design

#### Efficiency Factors

- **Latency**: 
- **Bandwidth**: 
- **Response Time**:
- **Throughput**: The system’s ability to handle high loads. 

#### Principles

1. **Replication**: Add more resources to handle the load
2. **Optimization**: Fine tune existing resources to ensure it is able to handle the load without the need to add more resources.

### Terms

#### Performance vs Scalability

#### Availability vs Scalability

As we scale our systems through replicating resources, we create multiple instances of services that can be used to handle requests from any users. If one of our instances fails, the others remain available. The system just suffers from reduced capacity due to a failed, unavailable resource.

#### Consistency vs Scalability

If our single database server becomes overloaded, we can replicate it and send requests to either instance. This also increases availability as we can tolerate the failure of one instance. This scheme works great if our databases are read only. But as soon as we update one instance, we somehow have to figure out how and when to update the other instance. This is where the issue of replica consistency raises its ugly head.

whenever state is replicated for scalability and availability, we have to deal with consistency.

### Security

The basic elements of a secure system are authentication, authorization, and integrity. We need to ensure data cannot be intercepted in transit over networks, and data at rest (persistent store) cannot be accessed by anyone who does not have permission to access that data.

#### Confidentiality

At the network level, systems routinely exploit the [Transport Layer Security (TLS) protocol](https://oreil.ly/pG2eg), which runs on top of TCP/IP. TLS provides encryption, authentication, and integrity using [asymmetric cryptography](https://oreil.ly/FqPSm). This has a performance cost for establishing a secure connection as both parties need to generate and exchange keys. TLS connection establishment also includes an exchange of certificates to verify the identity of the server (and optionally client), and the selection of an algorithm to check that the data is not tampered with in transit. Once a connection is established, in-flight data is encrypted using symmetric cryptography, which has a negligible performance penalty as modern CPUs have dedicated encryption hardware. Connection establishment usually requires two message exchanges between client and server, and is thus comparatively slow. Reusing connections as much as possible minimizes these performance overheads 

#### Integrity

There are multiple options for protecting data at rest. Popular database engines such as SQL Server and Oracle have features such as transparent data encryption (TDE) that provides efficient file-level encryption. 

#### Availability

Availability refers to a system’s ability to operate reliably under attack from adversaries. 

In general, **security and scalability** are opposing forces. Security necessarily introduces performance degradation. The more layers of security a system encompasses, then a greater burden is placed on performance, and hence scalability. This eventually affects the bottom line—more powerful and expensive resources are required to achieve a system’s performance and scalability requirements 

### Manageability

As the systems we build become more distributed and complex in their interactions, their management and operations come to the fore. We need to pay attention to ensuring every component is operating as expected, and the performance is continuing to meet expectations.

### DevOps

### Scale Out/Horizontal Scaling

Scaling out relies on the ability to replicate a service in the architecture and run multiple copies on multiple server nodes. Requests from clients are distributed across the replicas so that in theory, if we have N replicas and R requests, each server node processes R/N requests. This simple strategy increases an application’s capacity and hence scalability. 

#### Load Balancer

All user requests are sent to a load balancer, which chooses a service replica target to process the request. Various strategies exist for choosing a target service, all with the core aim of keeping each resource equally busy. The load balancer also relays the responses from the service back to the client. Most load balancers belong to a class of internet components known as [reverse proxies](https://oreil.ly/78lLN).

#### Stateless Services

For load balancing to be effective and share requests evenly, the load balancer must be free to send consecutive requests from the same client to different service instances for processing.

Unfortunately, as with any engineering solution, simple scaling out has limits. As you add new service instances, the request processing capacity grows, potentially infinitely. At some stage, however, reality will bite and the capability of your single database to provide low-latency query responses will diminish. Slow queries will mean longer response times for clients. If requests keep arriving faster than they are being processed, some system components will become overloaded and fail due to resource exhaustion, and clients will see exceptions and request timeouts. Essentially, your database becomes a bottleneck that you must engineer away in order to scale your application further. 

### Database

#### Distributed SQL 

##### NewSQL

NewSQL is a unique database system that combines ACID compliance with horizontal scaling. The database system strives to keep the best of both worlds. OLTP-based transactions and the high performance of NoSQL combine in a single solution.

#### Distributed NoSQL

NoSQL (Not SQL or Not Only SQL) is a generic term used for databases that do not depend on a relational model. The data does not need to have a strict schema nor the usual SQL table structure. Most commonly, the data is aggregated as **key-value pairs, JSON documents, graphs,** or **wide-column tables.**

By using NoSQL databases, you can store immense volumes of unstructured data as it comes in and structure it at a later point. As expected, this leads to much better throughput, read/write speeds, and allows you to scale out servers horizontally.

### Multiple Processing Tiers

In fulfilling a request, a service can call one or more dependent services, which in turn are replicated and load-balanced 

#### Backend for Frontend(BFF)

