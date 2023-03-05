---
layout: post
title: 'Networking: What is CIDR'
date: '2023-02-26 17:23:43 +0530'
categories: [NETWORKING]
tags: [networking, cidr, subnetting]
---

## What is CIDR

CIDR (Classless Inter-Domain Routing) is a way of assigning unique addresses to devices on a computer network.

CIDR uses a prefix notation to represent IP addresses and their associated subnet masks. The prefix indicates the number of bits in the subnet mask that are used to identify the network portion of the address. For example, a prefix of /24 indicates that the first 24 bits of the IP address are used to identify the network, while the remaining 8 bits are used to identify individual devices on the network. 

This allows for more efficient use of IP addresses and more flexibility in designing networks. For example, with CIDR, a network administrator can divide a large network into smaller subnets and assign unique IP addresses to each device on those subnets

### Example

10.0.0.0/**24**

**24**-bits are used to identify the network and the remaining 8-bits are used to identify the host.

#### Available IPs

The formula to calculate the number of assignable IP address to CIDR networks is "**Subtract the number of network bits from 32. Raise 2 to that power and subtract 2 for the network and broadcast addresses**". For example, a /24 network has 2^(32 - 24) - 2 addresses available for host assignment.

Some example ranges as per above formula:

| CIDR Notation | Available Bits | Host Formula | Available Hosts |
| ------------- | -------------- | ------------ | --------------- |
| /8            | 32 - 8 = 24    | 2^24 - 2     | 16,777,214      |
| /16           | 32 - 16 = 16   | 2^16 - 2     | 65,534          |
| /24           | 32 - 24 = 8    | 2^8 - 2      | 254             |
| /26           | 32 - 26 = 6    | 2^6 - 2      | 62              |
| /28           | 32 - 28 = 4    | 2^4 - 2      | 14              |
| /30           | 32 - 30 = 2    | 2^2 - 2      | 2               |

