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



So the IP address which we can use is 10.0.1.4 to 10.0.1.254