# Draft #1 - Tamto

## Abstract

This document proposes the design of a new, open-source messaging system written in Rust, a language chosen for its memory safety, speed, and energy efficiency - attributes that present a compelling alternative to Java-based systems like Apache Pulsar. The name of the proposed system is Tamto. Tamto aims to offer high performance, modularity, extensibility, and a focus on data processing functions. The system is built on top of the Chord protocol for distributed data management, a choice that further enhances its scalability and reliability. The core messaging pattern of the system is the publish-subscribe pattern, aimed at facilitating easy and efficient data transmission. By fusing these innovative design choices, this messaging system aims to push the boundaries of what is currently possible in the domain of event based systems.

<!-- toc -->

## 1. Introduction

The proposed messaging system is designed to be a high-performance alternative to existing solutions such as Apache Pulsar. Written in Rust, it takes advantage of the language's safety, concurrency, and performance features. The system's architecture is modular and extensible, allowing users to build custom functionality and data processing capabilities within the system.

## 2. System Architecture

Tamto consists of several key components, including:

 - **Messaging Broker**: Manages message routing and delivery between producers and consumers.
 - **Message Producers and Consumers**: Generate and process messages within the system.
 - **Chord Protocol Implementation**: Provides a distributed data management layer for efficient message routing and storage.
 - **In-memory Message Storage**: Stores messages in memory with optional disk storage for persistence.
 - **Data Processing Module**: Supports basic data processing capabilities and extensibility for additional functionality.
 - **Role-Based Access Control (RBAC)**: Manages authentication and authorization within the messaging system.

~~~admonish warning title="TODO"
Add image of system architecture here.
~~~

### 2.1 Messaging Broker

Messaging Broker is the core component of the system. It manages message routing and delivery between producers and consumers. The broker is built on top of the Chord protocol, ensuring efficient distribution and scalability of messages across the network.

~~~admonish question title="How should the message delivery be handled?"
The idea is to use the Chord finger tables to deliver messages to consumers. This ensures even distribution of messages across the network. Another advantage is that if the number of nodes increases, the number of node-to-node connections will not increase.

The question is what happens when there is a large amount of consumers for a single topic? How would the performance be affected? 

What other approaches could be considered for better performance?
~~~

### 2.2 Message Producers and Consumers

The broker receives incoming messages from producers via Cap'n'Proto streaming for persistent connections or Cap'n'Proto publish for one-off messages. Once a message is received, the broker routes it based on a subscription model. Consumers subscribe to specific topics and receive all messages published to those topics. Each consumer maintains an index to the last message it processed. This index handling allows multiple consumers to process all messages independently if desired, or share the index for exclusive processing where only one consumer receives each message.

### 2.3 Chord Protocol Implementation

Chord is a protocol for building decentralized data storage systems. It is used to efficiently locate the node that stores a specific piece of data in a network of nodes. This makes it a great fit for the scalable messaging system. In theory there is no limit to the number of nodes that can be added to the network, making it a great choice for a messaging system that needs to scale.

The detailed design of the Chord protocol can be found in the [Chord paper](https://pdos.csail.mit.edu/papers/ton:chord/paper-ton.pdf).

There are various components of Chord which could be used in Tamto for effective message routing and delivery. These include:

 - **Chord Finger Table**: A data structure that stores information about limited number of other nodes in the network. It is used to efficiently locate the node that stores a specific piece of data in a network of nodes.


### 2.4 In-memory Message Storage

Stores messages in memory with optional disk storage for persistence.

### 2.5 Data Processing Module

Supports basic data processing capabilities and extensibility for additional functionality.

### 2.6 Role-Based Access Control (RBAC)

Manages authentication and authorization within Tamto.


## 3. Messaging Protocols
Tamto will use Cap'n'Proto streaming for persistent connections and Cap'n'Proto publish for one-off messages.

### 3.1 Cap'n'Proto Streaming

### 3.2 Cap'n'Proto Publish

## 4. Message Routing and Delivery
Tamto will support consumer subscriptions based on topics. Each consumer will maintain an index to the last message it processed. This index can be either shared or separate, depending on whether multiple consumers need to process the same messages or not. Message routing will utilize Chord finger tables for efficient distribution.

### 4.1 Consumer Subscriptions

### 4.2 Message Indexing

### 4.3 Chord Finger Table

## 5. Message Storage
Tamto will store messages primarily in memory, with optional disk storage for persistence. Message retention time can be configured on a per-topic basis. When the retention time expires, messages are removed from memory and can be retrieved from disk if necessary.

### 5.1 In-memory Storage

### 5.2 Optional Disk Storage

### 5.3 Message Retention

## 6. Scalability and High-throughput Considerations
To handle high-throughput scenarios, Tamto can use partitioning to distribute topics across the cluster and replication to distribute messages among consumers. The system will be built on top of the Chord protocol, enabling it to scale efficiently as the number of nodes increases. For situations with many producers, the system can distribute the topic across the cluster using partitioning. If there are many consumers, the topic could be replicated, utilizing the Chord finger table to distribute messages effectively.

### 6.1 Partitioning

### 6.2 Replication

## 7. Error Handling and Data Integrity
The system will incorporate replication mechanisms to maintain multiple copies of data within the cluster, helping to handle error situations such as network failures or system crashes. Both global replication and topic-specific replication configurations will be used. To prevent or recover from data corruption, the system will employ a combination of robust error-checking and recovery mechanisms.

### 7.1 Replication

### 7.2 Data Corruption Prevention and Recovery

## 8. Security and Privacy
Security and privacy are fundamental in the system design. The broker uses Role-Based Access Control (RBAC) for authorizing consumers, ensuring that only authorized consumers receive messages from their subscribed topics. Additionally, TLS will be used for secure communication between nodes, consumers, and producers, guaranteeing the integrity and confidentiality of the transferred data.

### 8.1 Role-Based Access Control (RBAC)

### 8.2 TLS

## 9. Extensibility and Modularity
One of the core design principles of Tamto is its extensibility and modularity. While the specifics of this feature will be determined as the system evolves, the goal is to allow additional features or components to be added without disrupting its core functionality. This extensibility will enable the system to evolve and adapt to changing requirements and conditions over time.

### 9.1 Data Processing Module

### 9.2 Modular Architecture

## 10. Example Application: ???

~~~admonish warning title="TODO"
The following section is a placeholder for an example application that uses Tamto.

The example application should be a simple, real-world application that demonstrates the system's capabilities and how it can be used in practice.
~~~

## 11. Conclusion
Tamto system aims to bring together the efficiency and safety of Rust, the power of a distributed system using the Chord protocol, and the flexibility of a modular architecture. With its focus on high performance, scalability, security, and extensibility, it has the potential to become a strong contender in the messaging system space.

Further work is needed to refine the system's design, particularly around in-memory message storage and the integration of extensible modules. However, the foundational design principles and components proposed in this document provide a solid starting point for the system's development.