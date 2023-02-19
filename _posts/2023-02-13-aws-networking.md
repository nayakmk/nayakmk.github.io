---
layout: post
title: 'AWS: Networking'
date: '2023-02-13 07:47:22 +0530'
---

## AWS Networking

### Amazon Virtual Private Cloud(VPC)

Amazon Virtual Private Cloud (VPC) is a service that allows you to launch Amazon Web Services (AWS) resources into a virtual network that you've defined. A VPC is a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. This provides you with complete control over the virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

When creating a VPC, you need to consider the following:

1. **CIDR Block**: The VPC CIDR block is the IP address range for your VPC. You will specify the CIDR block when you create the VPC.

2. **Subnets**: You will need to create subnets within your VPC to separate the different parts of your application.

3. **Internet Gateway**: You will need to create an Internet Gateway if you want resources in your VPC to be able to access the Internet.

4. **Route Tables**: Each subnet must be associated with a route table, which determines the traffic routing for the subnet. 

   Further Info: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html

5. **Security Groups**: Security groups are used to control inbound and outbound traffic to and from your resources.

6. **Network ACLs**: Network ACLs are used to control traffic to and from subnets.

Once you have created a VPC, you can then launch AWS resources, such as Amazon EC2 instances, into your VPC. You can also configure network gateways, such as Virtual Private Gateways and VPN Connections, to allow communication between your VPC and other networks.

With a VPC, you can secure your AWS resources and ensure that only authorized traffic is allowed to access them. You can also connect your VPC to other networks, such as on-premises data centers or other VPCs, using VPN or AWS Direct Connect.

### Subnet

An Amazon Virtual Private Cloud (VPC) subnet is a logically isolated section of a VPC network. A subnet can be thought of as a segment of a VPC's IP address range, where you can place groups of isolated resources. Subnets are used to separate the different parts of an application, making it easier to manage and secure your resources.

When creating a VPC subnet, you need to consider the following:

1. **VPC CIDR Block**: The VPC CIDR block is the IP address range for your VPC. When you create a subnet, you will specify the CIDR block for the subnet, which must be a subset of the VPC CIDR block.
2. **Availability Zone**: You need to specify the Availability Zone in which the subnet will be created. Availability Zones are physically isolated sections of an AWS region that provide low latency and high throughput network connections.
3. **Public or Private**: You need to decide if the subnet will be public or private. *A public subnet is one that has a direct route to the Internet, while a private subnet does not.*
4. **Route Table**: Each subnet must be associated with a route table, which determines the traffic routing for the subnet.

![AWS Networking](/assets/img/AWS%20Concepts-Page-2.jpg)

**All subnets (regardless of whether they are Public or Private) within the same Amazon VPC can communicate with each other by default**. Communication should be made via the private IP address of the resources, to ensure that the traffic stays within the VPC. 

#### Public Subnet

A private subnet becomes public subnet when it's connected to an Internet Gateway. This allows both directional communication of components inside subnet to and from Internet.

| Destination | Target       |                                                              |
| ----------- | ------------ | ------------------------------------------------------------ |
| 10.0.0.0/16 | local        | This is needed to allow VPC connection.                      |
| 0.0.0.0     | igw-92dbb292 | This is needed to allow inbound and outbound internet connection from a subnet via a Internet Gateway. |

#### Private Subnet

A private subnet by default cannot access to Internet. In order for the private subnet to access the internet, it has to be rerouted via another public subnet. In order for the private subnet to allow the connection, it needs an entry in route table to a component called **NAT Gateway**, which resides inside public subnet.

| Destination | Target       |                                                              |
| ----------- | ------------ | ------------------------------------------------------------ |
| 10.0.0.0/16 | local        | This is needed to allow VPC connection.                      |
| 0.0.0.0     | nat-92dbb292 | This is needed to allow outbound internet connection from a private subnet via a NAT Gateway. |

### IP Ranges

When you use CIDR address in a subnet, this is how AWS distributes the IP for usage of further networking components.

Ex. 10.0.1.0/24

This gives a total of 256 IP address but we can only use 251 IP address. The remaining are used by AWS as follows.

a. 10.0.1.0 : Used for Network

b. 10.0.1.1 : Used for AWS Routing

c. 10.0.1.2 : Used for AWS DNS

d. 10.0.1.3 : Used for Future usage

e. 10.0.1.255 : Used for Broadcast



So the IP address which we can use is 10.0.1.4 to 10.0.1.254.

### Network Access Control List (NACL)

Amazon VPC Network Access Control Lists (NACLs) are an optional layer of security for your VPC that act as a firewall for controlling traffic in and out of one or more subnets. **NACLs provide an additional level of security for your resources by allowing you to define rules that control the traffic to and from your subnets**.

NACLs are stateless, which means that they do not track the connection state of incoming and outgoing network traffic, and they do not provide features such as load balancing or routing. Instead, NACLs simply allow or deny traffic based on the rules that you define.

NACLs operate at the **subnet** level, and you can associate one or more subnets with a NACL. Each **subnet must be associated with one and only one NACL**. When you create a subnet, it is automatically associated with the default NACL, which allows all traffic. You can then create custom rules for the NACL to control the traffic to and from the subnet.

When you create a NACL, you specify the rules for incoming and outgoing traffic. Each rule has a rule number, and the rules are processed in ascending order. You can create up to 20 inbound and 20 outbound rules per NACL.

To use NACLs, you simply create the NACL and define the rules, and then associate the NACL with the desired subnets. You can use the AWS Management Console, AWS CLI, or the AWS SDKs to create and manage NACLs.

NACLs are an important security tool for controlling access to your resources in a VPC. They complement security groups, which are stateful and provide more advanced features such as load balancing and routing. By using both NACLs and security groups, you can create a multi-layered security system for your VPC that provides robust protection for your resources.

#### Rules

Amazon VPC **Network Access Control Lists** (NACLs) use rules to control the traffic in and out of a VPC subnet. Each NACL rule consists of the following elements:

1. **Rule Number**: The rule number determines the order in which the rules are processed. Rules are processed in ascending order, starting with the lowest rule number.
2. **Rule Action**: The rule action specifies whether to allow or deny traffic that matches the rule.
3. **Protocol**: The protocol specifies the type of network traffic that the rule applies to, such as TCP, UDP, or ICMP.
4. **Port Range**: The port range specifies the range of TCP or UDP ports that the rule applies to.
5. **Source IP Range**: The source IP range specifies the range of IP addresses that are allowed to initiate traffic to the subnet. You can specify a specific IP address or a range of IP addresses.
6. **Target**: The target is the subnet associated with the NACL.

#### Default NACL

The **default network ACL** is configured to allow all traffic to flow in and out of the subnets with which it is associated. Each network ACL also includes a rule whose rule number is an asterisk. This rule ensures that if a packet doesn't match any of the other numbered rules, it's denied. You can't modify or remove this rule. 

| **Inbound**  |                  |              |                |                 |                |
| ------------ | ---------------- | ------------ | -------------- | --------------- | -------------- |
| **Rule #**   | **Type**         | **Protocol** | **Port range** | **Source**      | **Allow/Deny** |
| 100          | All IPv4 traffic | All          | All            | 0.0.0.0/0       | ALLOW          |
| *            | All IPv4 traffic | All          | All            | 0.0.0.0/0       | DENY           |
| **Outbound** |                  |              |                |                 |                |
| **Rule #**   | **Type**         | **Protocol** | **Port range** | **Destination** | **Allow/Deny** |
| 100          | All IPv4 traffic | All          | All            | 0.0.0.0/0       | ALLOW          |
| *            | All IPv4 traffic | All          | All            | 0.0.0.0/0       | DENY           |

#### Custom NACL - Example

| Inbound      |             |              |                |                 |                |                                                              |
| ------------ | ----------- | ------------ | -------------- | --------------- | -------------- | ------------------------------------------------------------ |
| **Rule #**   | **Type**    | **Protocol** | **Port range** | **Source**      | **Allow/Deny** | **Comments**                                                 |
| 100          | HTTP        | TCP          | 80             | 0.0.0.0/0       | ALLOW          | Allows inbound HTTP traffic from any IPv4 address.           |
| 110          | HTTPS       | TCP          | 443            | 0.0.0.0/0       | ALLOW          | Allows inbound HTTPS traffic from any IPv4 address.          |
| 120          | SSH         | TCP          | 22             | 192.0.2.0/24    | ALLOW          | Allows inbound SSH traffic from your home network's public IPv4 address range (over the internet gateway). |
| 130          | RDP         | TCP          | 3389           | 192.0.2.0/24    | ALLOW          | Allows inbound RDP traffic to the web servers from your home network's public IPv4 address range (over the internet gateway). |
| 140          | Custom TCP  | TCP          | 32768-65535    | 0.0.0.0/0       | ALLOW          | Allows inbound return IPv4 traffic from the internet (that is, for requests that originate in the subnet).This range is an example only. For more information about how to select the appropriate ephemeral port range, see [Ephemeral ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html?ref=wellarchitected#nacl-ephemeral-ports). |
| *            | All traffic | All          | All            | 0.0.0.0/0       | DENY           | Denies all inbound IPv4 traffic not already handled by a preceding rule (not modifiable). |
| **Outbound** |             |              |                |                 |                |                                                              |
| **Rule #**   | **Type**    | **Protocol** | **Port range** | **Destination** | **Allow/Deny** | **Comments**                                                 |
| 100          | HTTP        | TCP          | 80             | 0.0.0.0/0       | ALLOW          | Allows outbound IPv4 HTTP traffic from the subnet to the internet. |
| 110          | HTTPS       | TCP          | 443            | 0.0.0.0/0       | ALLOW          | Allows outbound IPv4 HTTPS traffic from the subnet to the internet. |
| 120          | SSH         | TCP          | 1024-65535     | 192.0.2.0/24    | ALLOW          | Allows outbound SSH traffic from your home network's public IPv4 address range (over the internet gateway). |
| 140          | Custom TCP  | TCP          | 32768-65535    | 0.0.0.0/0       | ALLOW          | Allows outbound IPv4 responses to clients on the internet (for example, serving webpages to people visiting the web servers in the subnet).This range is an example only. For more information about how to select the appropriate ephemeral port range, see [Ephemeral ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html?ref=wellarchitected#nacl-ephemeral-ports). |
| *            | All traffic | All          | All            | 0.0.0.0/0       | DENY           | Denies all outbound IPv4 traffic not already handled by a preceding rule (not modifiable). |

### Security Group

Amazon Web Services (AWS) security groups are used to control inbound and outbound traffic to and from AWS resources. Here are some options and features that are available in AWS security groups:

1. **Inbound and Outbound Rules**: Security groups allow you to create inbound and outbound rules to control traffic. You can specify which protocols, ports, and IP addresses are allowed to connect to your resources.
2. **Port Range**: You can specify a range of ports that are allowed or denied access for a specific protocol.
3. **IP Address Range**: You can specify the IP address range that is allowed or denied access to your resources.
4. **Protocol Type**: You can specify the protocol type (TCP, UDP, ICMP) that is allowed or denied access to your resources.
5. **Security Group ID**: You can allow or deny traffic from specific security groups that are associated with your resources.
6. **VPC**: You can create security groups within a VPC and control traffic between resources within that VPC.
7. **Inbound and Outbound traffic logs**: Security groups can log traffic that is allowed or denied, which can be useful for auditing and troubleshooting.
8. **Modifying Rules**: You can modify the rules of an existing security group at any time.
9. **Predefined rules**: AWS provides some predefined security group rules that you can use as a starting point for your own security groups.
10. **Network Interface**: You can associate a network interface with a security group to apply security rules to traffic that flows through that interface.

These are just some of the options available in AWS security groups. The specific options available to you may depend on the AWS service you are using and the type of resources you are protecting.

#### Example Rules

| **Inbound**            |              |                |                                                              |
| ---------------------- | ------------ | -------------- | ------------------------------------------------------------ |
| **Source**             | **Protocol** | **Port range** | **Description**                                              |
| `sg-1234567890abcdef0` | All          | All            | Allows inbound traffic from all resources that are assigned to this security group. The source is the ID of this security group. |
| **Outbound**           |              |                |                                                              |
| **Destination**        | **Protocol** | **Port range** | **Description**                                              |
| 0.0.0.0/0              | All          | All            | Allows all outbound IPv4 traffic.                            |
| ::/0                   | All          | All            | Allows all outbound IPv6 traffic. This rule is added only if your VPC has an associated IPv6 CIDR block. |

### NAT Gateway

A NAT (Network Address Translation) gateway is a service that enables instances in a private subnet to connect to the internet or other AWS services while preventing direct access from the public internet.

In Amazon Web Services (AWS), there are a few options for setting up a NAT gateway:

1. AWS-managed NAT Gateway: This is a fully managed NAT gateway service provided by AWS. It is easy to set up and can scale automatically to handle large amounts of traffic. However, it can be more expensive than other options, and there is no option to customize the gateway configuration.
2. Self-managed NAT instance: You can launch an EC2 instance in a public subnet, configure it as a NAT device, and route traffic through it to the internet. This option gives you more control over the configuration and can be less expensive than the AWS-managed option. However, it requires more management and maintenance than the managed option.
3. Third-party NAT appliance: You can use a third-party NAT appliance, such as Cisco ASAv or Fortinet FortiGate, in a public subnet to provide NAT services. This option gives you even more control and customization options, but it can be more complex to set up and manage.

Overall, the choice of NAT gateway option depends on your specific use case and requirements for control, customization, and cost.

### Bastion Hosts

In AWS, a bastion host is a special-purpose instance that is securely located in a public subnet and used to access and manage instances in private subnets. The bastion host acts as a jump server, allowing secure access to the instances in the private subnet from the internet or other public networks.

The main benefits of using a bastion host include:

1. Improved security: By limiting direct access to instances in the private subnet from the internet, a bastion host helps to protect against unauthorized access and attacks.
2. Simplified management: Using a bastion host can simplify the management of instances in the private subnet by providing a single access point for administration and troubleshooting.
3. Enhanced auditing and logging: Because all access to instances in the private subnet is routed through the bastion host, it can provide a centralized location for auditing and logging of all access activity.

AWS provides a number of options for setting up a bastion host, including using an EC2 instance or a managed service like AWS Systems Manager Session Manager or AWS PrivateLink. The specific implementation will depend on your use case and requirements for security, management, and performance.

### VPN 

In AWS, a VPN (Virtual Private Network) connection is a secure connection between your on-premises network or data center and your Amazon VPC (Virtual Private Cloud) that allows you to securely access resources in your VPC as if they were part of your local network.

The main benefits of using a VPN connection in AWS include:

1. Improved security: A VPN connection provides a secure, encrypted tunnel between your on-premises network and your VPC, helping to protect your data and resources from unauthorized access.
2. Increased connectivity: A VPN connection enables you to securely access resources in your VPC from your on-premises network or data center, making it easier to manage and integrate your resources across environments.
3. Reduced costs: Using a VPN connection can be more cost-effective than using a dedicated network connection or a direct connect service, particularly for smaller organizations.

AWS provides a number of options for setting up a VPN connection, including using a customer gateway device on your on-premises network or data center and configuring a virtual private gateway in your VPC. You can also use a managed service like AWS Site-to-Site VPN or AWS Client VPN to simplify the setup and management of your VPN connection.

The specific implementation of a VPN connection will depend on your use case and requirements for security, performance, and connectivity.

### Direct Connect

In AWS, Direct Connect is a dedicated network connection service that provides a private and dedicated connection between your on-premises network or data center and your Amazon VPC (Virtual Private Cloud). Direct Connect enables you to bypass the public internet and establish a secure and high-bandwidth connection to your AWS resources.

The main benefits of using Direct Connect in AWS include:

1. Enhanced performance: Direct Connect provides a dedicated, high-bandwidth connection between your on-premises network and your VPC, enabling faster and more consistent network performance.
2. Improved security: Because Direct Connect establishes a private and dedicated connection, it can help to enhance the security of your data and resources by bypassing the public internet.
3. Greater reliability: Direct Connect provides a highly available and redundant connection, with multiple physical connections and automatic failover to ensure reliable connectivity.

AWS provides a number of options for setting up Direct Connect, including using a Direct Connect Partner to establish the connection or setting up a dedicated connection through a colocation facility. You can also use a managed service like AWS Direct Connect Gateway to simplify the setup and management of your Direct Connect connections.

The specific implementation of Direct Connect will depend on your use case and requirements for performance, security, and reliability.

### VPC Peering

In AWS, VPC peering is a service that allows you to connect two or more VPCs in the same region and route traffic between them privately and securely. VPC peering enables you to extend your network across multiple VPCs, simplifying management and enabling you to build more complex and distributed applications.

The main benefits of using VPC peering in AWS include:

1. Improved connectivity: VPC peering allows you to connect VPCs in the same region as if they were on the same network, enabling you to access resources across VPCs as if they were local.
2. Enhanced security: VPC peering routes traffic between VPCs privately and securely, without the need for internet gateways or VPN connections, helping to ensure the security of your resources and data.
3. Simplified management: VPC peering enables you to manage multiple VPCs as a single network, simplifying network administration and reducing the complexity of your infrastructure.

AWS provides a number of options for setting up VPC peering, including using the AWS Management Console, the AWS CLI, or the AWS SDKs. You can also use a managed service like AWS Transit Gateway to simplify the management of multiple VPC peering connections.

The specific implementation of VPC peering will depend on your use case and requirements for security, performance, and connectivity. It is important to note that VPC peering connections cannot span across multiple regions.

### Transit Gateway

In AWS, a Transit Gateway is a managed service that allows you to connect multiple Amazon VPCs, on-premises networks, and AWS accounts together in a centralized hub-and-spoke architecture. Transit Gateway simplifies network connectivity by providing a single gateway for routing traffic between VPCs and external networks.

The main benefits of using Transit Gateway in AWS include:

1. Simplified network architecture: Transit Gateway enables you to connect multiple VPCs and on-premises networks together in a single hub-and-spoke architecture, reducing the complexity of your network infrastructure.
2. Enhanced scalability: Transit Gateway supports up to 5,000 VPC attachments, enabling you to easily scale your network as your needs grow.
3. Improved performance: Transit Gateway provides optimized routing between VPCs and on-premises networks, enabling faster and more consistent network performance.
4. Enhanced security: Transit Gateway allows you to control network traffic with security groups and network access control lists (ACLs), providing enhanced security for your network resources.

AWS provides a number of options for setting up Transit Gateway, including using the AWS Management Console, the AWS CLI, or the AWS SDKs. Transit Gateway is a pay-as-you-go service, and pricing is based on the amount of data processed and the number of attachments.

The specific implementation of Transit Gateway will depend on your use case and requirements for security, scalability, and performance. Transit Gateway supports various use cases such as centralized routing, shared services, and hybrid cloud deployments.

### Route 53

In AWS, Route 53 is a managed DNS (Domain Name System) service that provides global routing and management for your domain names. Route 53 enables you to route end users to your AWS resources, such as EC2 instances, S3 buckets, and load balancers, based on the geographic location of the user or other criteria.

The main benefits of using Route 53 in AWS include:

1. Scalability: Route 53 can handle millions of queries per second, making it a highly scalable service that can meet the needs of even the most demanding applications.
2. High availability: Route 53 is designed for high availability, with multiple geographically dispersed DNS servers and automatic failover capabilities.
3. Customizable routing policies: Route 53 provides a range of routing policies that enable you to route traffic based on geographic location, latency, health of the resource, and other criteria, enabling you to optimize your application's performance and availability.
4. Integration with other AWS services: Route 53 integrates with other AWS services, such as CloudFront, Elastic Load Balancing, and AWS Certificate Manager, enabling you to use these services together to build scalable and highly available applications.

AWS provides a number of options for setting up and managing Route 53, including using the AWS Management Console, the AWS CLI, or the AWS SDKs. Route 53 is a pay-as-you-go service, and pricing is based on the number of hosted zones, the number of queries processed, and other factors.

The specific implementation of Route 53 will depend on your use case and requirements for scalability, availability, and performance. Route 53 is a key component of many AWS architectures and is commonly used for DNS management and global routing of traffic.

#### Route Policies

In AWS Route 53, there are several routing policies available that enable you to route traffic to your resources based on various criteria. Here are the most common routing policies:

1. **Simple Routing**: With Simple Routing, you can route traffic to a single resource, such as an IP address, an EC2 instance, or a load balancer. This routing policy is ideal for single-resource scenarios and can be useful for testing and development environments.
2. **Weighted Routing**: Weighted Routing enables you to distribute traffic across multiple resources based on their weight. You can specify the percentage of traffic that should be routed to each resource, enabling you to perform A/B testing, gradually roll out new features, or route traffic to resources with different capabilities.
3. **Latency-Based Routing**: With Latency-Based Routing, you can route traffic to the resource that provides the lowest latency for the user. This routing policy is particularly useful for applications that require low latency and high performance.
4. **Failover Routing**: Failover Routing enables you to route traffic to a secondary resource in the event of a failure of the primary resource. You can configure Route 53 to monitor the health of your resources and automatically failover to a backup resource if the primary resource becomes unavailable.
5. **Geolocation Routing**: With Geolocation Routing, you can route traffic based on the geographic location of the user. You can specify different resources for different geographic regions, enabling you to provide localized content and improve the performance of your application.
6. **Geoproximity Routing**: Geoproximity Routing enables you to route traffic based on the geographic location of your resources and the user. You can specify different routing policies for resources that are close to the user or far away from the user, enabling you to optimize performance and reduce latency.
7. **Multi-Value Answer Routing**: Multi-Value Answer Routing enables you to return multiple values for a single DNS query. This routing policy is useful for load balancing and can help improve the availability and performance of your application.

Each routing policy has its own use case, and you can combine them to create complex routing scenarios based on your needs.

### AWS CloudFront

AWS CloudFront is a content delivery network (CDN) service that accelerates the delivery of static and dynamic web content, including images, videos, applications, and APIs. CloudFront is designed to provide low latency, high transfer speeds, and improved security for your content, while also reducing the load on your origin servers.

The main benefits of using CloudFront in AWS include:

1. Global reach: CloudFront uses a global network of edge locations to cache and deliver your content, enabling faster delivery to users around the world.
2. Improved performance: CloudFront can cache content at the edge locations, reducing the latency and improving the performance of your application.
3. Cost-effectiveness: CloudFront can help reduce the load on your origin servers and reduce your bandwidth costs by caching content at the edge locations.
4. Security: CloudFront provides a range of security features, including SSL/TLS encryption, custom SSL certificates, and AWS WAF (Web Application Firewall) integration, enabling you to protect your content and your users.

AWS provides a number of options for setting up and managing CloudFront, including using the AWS Management Console, the AWS CLI, or the AWS SDKs. CloudFront is a pay-as-you-go service, and pricing is based on the amount of data transferred and other factors.

The specific implementation of CloudFront will depend on your use case and requirements for performance, scalability, and security. CloudFront is commonly used for web and mobile applications, video streaming, and content delivery, and can be integrated with other AWS services, such as S3, EC2, and Lambda.