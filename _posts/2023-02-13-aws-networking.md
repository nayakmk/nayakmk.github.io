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

Amazon VPC Network Access Control Lists (NACLs) use rules to control the traffic in and out of a VPC subnet. Each NACL rule consists of the following elements:

1. Rule Number: The rule number determines the order in which the rules are processed. Rules are processed in ascending order, starting with the lowest rule number.
2. Rule Action: The rule action specifies whether to allow or deny traffic that matches the rule.
3. Protocol: The protocol specifies the type of network traffic that the rule applies to, such as TCP, UDP, or ICMP.
4. Port Range: The port range specifies the range of TCP or UDP ports that the rule applies to.
5. Source IP Range: The source IP range specifies the range of IP addresses that are allowed to initiate traffic to the subnet. You can specify a specific IP address or a range of IP addresses.
6. Target: The target is the subnet associated with the NACL.

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

| **Rule #**   | **Type**    | **Protocol** | **Port range** | **Source**      | **Allow/Deny** | **Comments**                                                 |
| ------------ | ----------- | ------------ | -------------- | --------------- | -------------- | ------------------------------------------------------------ |
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