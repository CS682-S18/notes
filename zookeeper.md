Discovery, Group Membership, and Zookeeper
==========================================

### Overview

A fundamental problem in distributed systems is *discovering* other nodes or services to be used by an application, and a very related problem is maintaining a list of nodes that are members of the *group* of nodes participating in an application.

The [Domain Name System (DNS)](https://en.wikipedia.org/wiki/Domain_Name_System) is one way a host is able to find the location, or IP address, of another host. 

In some cases, nodes may also be statically configured with a list of other nodes or services, however this approach has the limitation that if a host or service fails then then static list becomes inaccurate.

### ZooKeeper

> ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services.

ZooKeeper is designed to be simple, fast, and reliable. Many of its main features lie in how it is implemented under the hood. Over the semester we will discuss in more detail how these algorithms work and why they are important. This document focuses on how to use ZooKeeper in an application.

ZooKeeper works similar to a standard file system.

![fs](http://zookeeper.apache.org/doc/current/images/zknamespace.jpg)


A data node in ZooKeeper is a *znode*. These were designed to store coordination data such as node status or configuration information.

The guarantees provided by ZooKeeper are as follows:

> - Sequential Consistency - Updates from a client will be applied in the order that they were sent.
- Atomicity - Updates either succeed or fail. No partial results.
- Single System Image - A client will see the same view of the service regardless of the server that it connects to.
- Reliability - Once an update has been applied, it will persist from that time forward until a client overwrites the update.
- Timeliness - The clients view of the system is guaranteed to be up-to-date within a certain time bound.

### ZooKeeper for Group Membership

We will explore is using ZooKeeper as a way to discover other nodes participating in a particular application, for example a chat application. The basic idea is that the we will define a structure such that the top-level znode represents the application, each child of that znode represents a group member, that is a single instance of the application, and the data will contain all of the relevant details for communicating with the group member.

```
/CS682_chat
			
			 /member1
			 		{ip_addr:<value>, port:<value>} 
			 /member2		
			 		{ip_addr:<value>, port:<value>} 
```

### Getting Started

ZooKeeper is running on microcloud node mc01 at port 2181. You may also download [ZooKeeper](https://zookeeper.apache.org/) and run a server locally to use for your testing purposes. I recommend using version 3.4.11.

Recall that the microcloud nodes are not accessible directly from outside the CS firewall. You may use an ssh tunnel to connect to the class microcloud server if you are off campus, for example as follows:
`ssh -L 9000:mc01.cs.usfca.edu:2181 srollins@stargate.cs.usfca.edu`.

You may add a maven dependency as follows to use the Java API from your Java code. Beware that it is a bit finicky. I had to force maven to update dependencies from the command line.

```xml
<dependencies>
  <dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.11</version>
  </dependency>
</dependencies>
```
