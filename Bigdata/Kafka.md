- publish-subscribe messaging rethought as a distributed commit log
- Apache Kafka is an ==**event streaming platform**== used to collect, process, store, and integrate data at scale. 
- **Real time** streaming data for real time analytics
- Developed in Java
- Open Source 2010 using GitHub
- provides a ACID guarantees


### Background
#### Traditional system
Many use case for **publish/subscribe** start out the same way: with a simple **message queue** or inter-process communication channel. 
- so the application that needs to send monitoring information somewhere, so you open a direct connection from your application to an app that displays your metrics on a dashboard, and push metrics over that connection.
#### Traditional Architecture
- Batch oriented => too slow
- point in-time delivery
- point in-time updates
- Large window of time to process =>Keeps increasing => hard to find what's the current status of user order
- Growing need for quicker updates

#### BUT Kafka is
- is not a messaging system, note that this is an **event streaming platform.**
- **To simplify, you clean things up** 
  real time data, scalable and fault-tolerant platform
- **Central Repository and Coordination Server**
  Integration of different sources and sinks ( NoSQL and big data infrastructures )
- **Publish Data** 
  Defined from source applications
- **Subscribe**
  To what data you want
- Reduce - **Duplication, Errors, Inconsistency, Network Traffic, etc**
- **Decoupled, Scalable, highly available streaming microservices**


### Architecture 
- Event streaming platform is used to collect, process, store, and integrate data as scale.
- **Event** 
  any type of action, incident, or change that's identified or recorded by software.
  (a payment, a website click ...)
- **Kafka unit of Data**
  -it is called a ==Message==, and each message represents ==an Event==.
  -A Message is simple an array of bytes
  -<key, value>
  -Key has no real meaning but is used to sequence/stamp message, and for partition the data
  -Value is just a set of bytes to be decoded at a later time (by Consumer)
  
### Key Terms/concepts
- **Producer** - Create the Events
- **Consumer** - Read, uses, does something with the Events
- **Broker** - The Engine through which the Events pass. Manages, Stores, Coordination, Replication, etc, of Events. Distributed nodes.
- **Topics** - Is a way of managing and organising Events. Is a Log of Events of a certain Topic etc. page views, logins, clicks on a webpage, error message, etc. Separate Topic for each
- **Partitions** - Takes a Topic and breaks it into multiple logs (Partition). Each partition can reside on a different node. Allows for scalability by distributing the Events and allowing multiple Consumers. Events could be read out of sequence. The work of storing messages, writing new messages, and processing existing messages can be split among many nodes in the cluster.
- **Kafka Connect** - Predefined interfaces/APIs to connect to existing applications to Kafka. Avoids duplication of code. 

### Event and Log/Topics
- retain at least the last known value for each message key within the log of data for a single topic partition.
- Logs are **immutable** ( No deletes!! )
- Topics are **Durable**
- Topic == orders
- Configure life time of events, by Time Period, or size of log
- Any changes ( updates, deletes, etc) => A new Event is created and appended to the Log

### Event
- Something has happened
- **<k,v>**
- it's just a **series of bytes**
- Many different formats are possible
- Some can create Large string of bytes
- **Schema-Registry** 
  -set schema for data types producers and consumer use.
  -Everything consists of bytes, but it has no concept of what these bytes represent. 
  -Producers and consumers will work with the schema to ensure all payload conform to a specific schema
  -Producer do not need to include the schema metadata within the message since the schema information is retrieved from the Schema Registry
  -Consumer retrieve the schema information to sudeserialize the message payload correctly.

### How it works - brokers
- Is a computer, instance or container running the Kafka process
- Distributed Nodes, Scalability, Durability, availability
- Can have many Brokers on one Physical Server ( Virtualization )
- Each broker hosts some set of partitions and handles incoming requests to write new events to those partitions or read events from them.
- Brokers also handle replication of partitions between each other.

### How it Works – Topic Offset

- Consumer will start with Event=0
- Offset will progress to next Event
- One by One
- Updates Kafka Broker with new offset
- What if Consumer fails 
	-Kafka has last Offset value where consumer restart
	-Consumer will restart at next Offset automatically 
- But if you create a New Consumer on same Topic it will start at Offset=Zero 
  (This value can be changed by starting Consumer)

### Multiple Topcis
- Topics help you to organise events and how these topics can be used
- Create different Topics for different types of events - Multiple Topics
- Can duplicate data across Topics
	-Depend on the use ( Some events might require specific actions
	-Same event sent to different Topics
- Example : Order Event topic / Product Events Topic / User Events Topic ... 

### Partitions
- A Topic can become a **bottleneck**
	-As more and more data is added
	-And more Consumer want to read from it
- Partitioning takes the single Topic(log) and breaks it into multiple logs
	-Each Partition(of the Topic) can live on a separate node in the Kafka cluster.
	-The work of storing messages, writing new message, and processing existing messages can be split among many nodes in the cluster.
- Partitions can be replicated across nodes -> Durability & Availability
- How are messages allocated to a Partition
	-All partitions will get an event share of the data/events
	-When a key<k,v> exists, the destination partition is computed from a hash of the key.  This allows Kafka to guarantee that messages having the same key always land in the same partition, and therefore are always in order.

### Multiple Consumers & Consumer Groups
- A **Consumer Group,** consists of one or more Consumers that work together to consume a **Topic**
- Consumers can **horizontally scale** to consume Topics with a large number of messages.
- Adding more Consumers than Partitions, will cause some Consumer to be idle
- Consumers send **heart-beats** back to Broker
	-If no heart-beat (after X period) Partitions are reassigned
- Consumer Groups can be assigned to different tasks/applications
- Consumers can horizontally scale to consume Topics with a large number of messages.
- Care is needed in deciding number of
   ( Topics, Partitions per Topic, Consumers, Consumer Group)

### Kafka Connect
- A **Data Integration Framework** for Apache Kafka
- Developers tend to build the same bits of functionality over and over again around core Kafka
tasks
- Pre-Built framework / Connectors to other Environments
- Allows you to build **Data Pipelines**
- Code that does important work but is not tied in any way to the business you’re actually in
- Runs separate/independent to the Broker – Can be on different nodes/hardware
- Similar multi-Node configuration to Kafka Broker(s)
- **Connect Worker** – runs 1 or more **(Source) Connectors**, to connect with Source and reads data (**Producer**) from this external source, and produces/writes it to a Topic (runtime JAR file on Source)
-  **Sink Connector** – connects with **Target** and writes the (Topic) events (acts as a **Consumer**)

### Ksql DB
- KsqlDB is a Database for building real-time applications using streaming processing
- **Separated Distributed Compute** (ksqlDB) from **Storage** (Kafka)
- Using light-**weight SQL**
- Results can be used **downstream**
- Can **query Topics, Streams, Connectors** etc
- CommandLine and Web interfaces (plus others)
- Wraps up many steps of Kafka tech env under ksqlDB





### References
- https://kai-waehner.medium.com/is-apache-kafka-a-database-ddc310898f5c
- https://medium.com/event-driven-utopia/understanding-kafka-topic-partitions-ae40f80552e8
- https://developer.confluent.io/?_ga=2.30459071.1205823098.1646760182-1928664745.1644852518&_gac=1.191200472.1644855069.Cj0KCQiAmKiQBhClARIsAKtSj-nmKTA4BYK8vXrTeDoGuDFiYvWgSrRt4jw5VyZ91XvoD206XaKVvosaAss1EALw_wcB
- https://github.com/pmoskovi/kafka-learning-resources