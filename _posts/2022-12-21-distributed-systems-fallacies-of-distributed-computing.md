---
layout: post
title: 'Distributed Systems: Fallacies of Distributed Computing'
date: '2022-12-21 09:50:32 +0530'
categories: [DISTRIBUTED]
tags: [fallacies, patterns]
---

### Fallacies of Distributed Computing

#### 1.1    The network is reliable

The fallacy of "the network is reliable" refers to the assumption that a network will always be available and able to transmit data without interruption. However, in reality, networks can be unreliable due to various issues such as packet loss, delays, and outages.

There are several factors that can contribute to the reliability of a network, including:

1. Network infrastructure: The quality and condition of the network infrastructure, such as cables and routers, can impact the reliability of the network.
2. Network congestion: If a network is heavily used, it may become congested, leading to slower performance or even connection failures.
3. Interference: External factors such as electromagnetic interference or physical obstructions can impact the reliability of a wireless network.
4. Maintenance: Regular maintenance and upkeep of the network can help to ensure its reliability.

Therefore, it is important to consider the reliability of a network when designing and implementing distributed systems, as it can have significant impacts on the performance and functionality of the system.

##### Solution

* Retry and Ack Pattern
* Store and Forward Pattern
* Use reliable messaging Pattern

#### 1.2    Latency isn’t a problem

*Latency refers to the time it takes for a message to be transmitted between two devices on a network*. It is often measured in milliseconds (ms).

The fallacy of "latency not being a problem" refers to the assumption that latency will not be a significant factor in the performance of a distributed system. However, in reality, latency can be a significant factor in certain situations.

For example, in a distributed system that relies on real-time communication, such as a video conferencing application, high latency can lead to a poor user experience. Similarly, in a distributed system that relies on rapid communication between devices, such as a distributed database, high latency can affect the performance and reliability of the system.

There are several factors that can contribute to latency in a distributed system, including:

1. Physical distance: The further apart devices are, the longer it may take for a message to be transmitted between them.
2. Network congestion: If a network is congested, it may take longer for a message to be transmitted.
3. Protocols: Some protocols may be more efficient at transmitting data than others, which can impact latency.

Therefore, it is important to consider latency when designing and implementing distributed systems, as it can have significant impacts on the performance and functionality of the system.

##### Solution

* Avoid lot of inter-service communication
* Utilize CDN, Caching
* Avoid lot of network calls to carry out an operation in a micro service.

#### 1.3    Bandwidth isn’t a problem

As mentioned previously, bandwidth refers to the amount of data that can be transmitted over a network connection in a given amount of time. It is often measured in bits per second (bps) or megabits per second (Mbps).

The fallacy of "bandwidth not being a problem" refers to the assumption that the amount of bandwidth available will always be sufficient to support the demands being placed on the network. However, in reality, bandwidth can be a limiting factor in certain situations.

For example, if a network connection has a low bandwidth, it may not be able to support streaming high-definition video or transferring large files quickly. In these cases, the network may become congested, leading to slow performance or even connection failures.

Additionally, the distance between devices can also affect the amount of bandwidth available. The further apart two devices are, the weaker the signal may be, which can result in a lower bandwidth.

Therefore, it is important to consider bandwidth when designing and planning networks, as it can be a limiting factor in certain situations.

##### Solution

* Transmit the data that is relevant 
* Do Lazy loading of data from ORM

#### 1.4    The Network is Secure

The fallacy of "the network is secure" refers to the assumption that a network is immune to security threats such as hacking and spoofing. However, in reality, no network is completely secure and it is important to take steps to protect against potential security threats.

There are several measures that can be taken to secure a network, including:

1. Implementing strong passwords and using two-factor authentication to protect against unauthorized access.
2. Regularly updating software and firmware to address vulnerabilities.
3. Using encryption to protect data transmitted over the network.
4. Implementing firewalls to block unwanted traffic and protect against external threats.
5. Using network monitoring tools to detect and respond to potential security threats.

Overall, it is important to recognize that no network is completely secure and to take steps to protect against potential security threats.

#### 1.5    The Topology won’t change

The fallacy of "the topology won't change" refers to the assumption that the structure of a distributed system will remain constant over time. However, in reality, the topology of a distributed system can change due to various factors such as device failures, network reconfigurations, and new devices being added to the system.

It is important to design distributed systems to be resilient to topology changes and able to adapt to new configurations. This can include using protocols and algorithms that can handle changes in the topology, as well as implementing monitoring systems to detect and respond to changes in the system.

Additionally, it is important to consider the impact of topology changes on the performance and functionality of the system. For example, if a device failure results in a change in the topology, it may affect the communication between other devices in the system.

Overall, it is important to recognize that the topology of a distributed system can change and to design systems and processes accordingly to ensure the reliability and stability of the system.

#### 1.6    The admin will know what to do

The fallacy of "the administrator will know what to do" refers to the assumption that an administrator of a distributed system will always be able to handle any issues or problems that arise. However, in reality, administrators can encounter unexpected issues or situations that they may not be prepared to handle.

It is important to design distributed systems with robustness and fail-safes in place to protect against potential issues or failures. This can include measures such as redundant components, monitoring systems, and procedures for handling unexpected events.

Additionally, it is important to ensure that administrators have the necessary knowledge, skills, and resources to manage and maintain the system effectively. This can include providing ongoing training and support to ensure that administrators are able to handle various situations that may arise.

Overall, it is important to recognize that administrators are not infallible and to design systems and processes accordingly to ensure the reliability and stability of the system.

#### 1.7    Transport cost isn’t a problem

The fallacy of "transport cost not being a problem" refers to the assumption that the cost of transmitting data between devices on a network is not a significant consideration. However, in some cases, the cost of transmitting data can be a significant factor in the design and operation of a distributed system.

There are several factors that can contribute to the transport cost of a distributed system, including:

1. Distance: The further apart devices are, the more expensive it may be to transmit data between them.
2. Bandwidth: High-bandwidth connections can be more expensive to maintain than low-bandwidth connections.
3. Protocols: Some protocols may be more efficient at transmitting data than others, which can impact the transport cost.
4. Network infrastructure: The type of network infrastructure used can also affect the transport cost, as certain types of infrastructure may be more expensive to maintain than others.

Therefore, it is important to consider transport cost when designing and implementing distributed systems, as it can have significant impacts on the performance and cost-effectiveness of the system.

#### 1.8    The network is homogeneous

The fallacy of a homogeneous network refers to the assumption that all devices on a network are the same and have the same capabilities. This fallacy can lead to incorrect assumptions about the capabilities of the network and the devices connected to it. 

In reality, networks are often heterogeneous, meaning that they consist of a variety of different types of devices with different capabilities and configurations. This can include devices with different operating systems, hardware, and software.

It is important to consider the heterogeneity of a network when designing and implementing distributed systems, as it can have significant impacts on the performance and functionality of the system. For example, if a system is designed with the assumption that all devices are the same, it may not work correctly on devices with different configurations or capabilities.

Additionally, heterogeneity can also introduce security vulnerabilities and make it more difficult to manage and maintain the network. Therefore, it is important to consider the diversity of devices on a network and design systems and protocols that can accommodate these differences.

### Further Reading

https://architecturenotes.co/fallacies-of-distributed-systems/
https://www.linkedin.com/pulse/distributed-system-design-patterns-irfan-muhammad/?trk=pulse-article