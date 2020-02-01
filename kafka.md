## Kafka

* Uses distributed commit log - failures and reprocessing can be embraced
* Makes it easy to start with a few fields and grow from there
* Producers can help with CQRS nature of microservices
* Embraces async/message driven architecture

### Usecases

* Log aggregation - analyzing and aggregating log information
* Storing events
* External distributed commit logs
* Stream processing framework

### Architecture Overview

* Broker - heart of Kafka, this is where the data resides (referred as Kafka Cluster)
* Producer - sends the data as messages (standard is AVRO)
* Messages - each message assigned to a topic - the topic can be split up (partitioned) into separate logs, each partition has ordered data, that's how it can be read back in order
* Kafka's messages are only appended - each message is given an offset, that's how the consumer can point back anywhere in time to find the latest read message
* Topic can specify how many replicas they want to use - this (obviously) impacts how much storage we need
* Zookeeper - a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. (Selects leader)  - Zookeeper works in a group.
* Consumers (group of those) reading and processing messages.

### Broker
* Heart (hub) of data
* Built as a single server or a cluster
* How many brokers? - depends on the data, number of consumers = consumers + replication factor

### Zookeeper
* One server is elected as leader
* Zookeeper is ran as a cluster
* 5 servers is a sweet spot for zookeeper
* Traffic between Zookeeper and Broker is minimal
* One Zookeeper can oversee more than one cluster:
`zookeeper.connect = host:port/KafkaCluster1` (KafkaCluster1 is chroot)
* Every time a broker starts up, it registers itself in Zookeeper.
* Controller (leader) is the first element from the Cluster

### Replication
* Replication is leader-based
* Rest of the cluster is a consumer of that leader
* Each replica will request a full messages at a time
* If the replicate does not sink for >10 s, then it considers the leader that follower to be "out of sync".
* If a broker leaves, than the closest follower will be the leader
* Kafka uses a round-robin mechanism to pass leaders
* Replication is evenly distributed among cluster instanceswith the goal of highest available uptime

### Data Retention
* can be sized, time-based or combination of two
* controller via configuration set at broker level

### Kafka Topics
* Central Kafka Abstraction (Named feed or category of messages)
* Logical entity - spans across clusters
* Physically represented as a log
* Can span multiple brokers
When a producer sends a message to a topic, it's appended to a time ordered sequencial stream. Each message represents an event or fact that from the perspective of the producerit's worthwhile to persist. Messages are immutable.
It is the job of the consumer to reconcile between messages.

### Message Content
Each message has a:
* Timestamp
* Referencable Identifier
* Payload (binary)
The consumers read messages from a topic. Message consumption is possible from independent unlimited number of consumers.

### Message offset
* It's like a bookmark, "last read message".
* Maintained by the Kafka Consumer.
* Corresponds to the message identifier

### Message Retention Policy
* Retains all published messages regardless of consumption.
* Retention period is configurable (default is 168 hours or 7 days)
* Retention period is defined on a per topic basis

### Transaction or Commit Logs
* Source of truth
* Physically stored and maintained
* Higher-order data structure derive from the log
  - Tables, indexes, views, etc.
* Point of recovery
