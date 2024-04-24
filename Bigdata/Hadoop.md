
# Hadoop & Big Data

### **What is Big Data**
- Big data is when the size of the data itself becomes part of the problem
- Not every company will benefit from Big Data, **it depends on your size and your ability**
- Everything we do is increasingly leaving a digital trace which we can use and analyse


### **Hadoop**
- programming models to process large data sets
- Process Big Data on **clusters of commodity hardware**
- be written in Java
- built on Hadoop clusters, which are collections of computers or notes, that work together to execute computations on data

#### Scaling issue
- It is harder and more expensive to scale-up ( It depends needs to be applied )
	-Add additional resources to an exisitng node(CPU, RAM)
	-New units must be purchases if required resources can not be added
	-Also known as scale vertically
	**But, Things are changing here : Physically and in-Cloud**
- Scale-Out
	-Add more nodes/machines to an existing distributed application
	-Software Layer is designed for node additions or removal
	-Hadoop takes this approach: **A set of nodes are bonded together as a single distributed system**
	-Very easy to scale down as well

##### Principles
- Scale-Out rather than Scale-Up
- Bring code to data rather than data to code
- Abstract complexity of distributed and concurrent application
- Self managing
- Auto parallel processing


### Big Data / Hadoop Cluster
- A set of cheap commodity hardware 
- Networked together
- Resides in the same location
	-set of servers in a set of racks in a data center
- Cheap Commodity Server Hardware
	-No need for super-computers, use commodity unreliable hardware
	-Not desktops

### Abstracting Complexity
- Distributed Computing is HARD WORK
- Hadoop abstracts many complexities in distributed and concurrent applications
	-Defines small number of components
	-Provides simple and well defined interfaces of interactions between these components
- **Frees** developer from worrying about **system level challenges**
	-race conditions, data starvation
	-processing pipelines, data partitioning, code distribution, etc.
	-**Allows developers to focus on** application development and **business logic**



### Hadoop vs RDBMS
- **It Depends**
- Hadoop != RDBMS
- Hadoop will not replace RDBMS
- Hadoop is part of your data management architecture and only if it is needed

#### Working together
- Hadoop and RDBMS frequently complement each other within an architecture
- For example a website that has a small number of users and produces a large amount of audit logs
	1. Utilize RDBMS to provide rich User interface and enforce data integrity
	2. RDBMS generates large amounts of audit logs, and the logs are moved periodically to the Hadoop cluster
	3. All logs are kept in Hadoop; Various analytics are executed periodically
	4. Results copied to RDBMS to be used by Web Server


### Hadoop Distributions
- Large number of independent product
	-Can be challenging to get all/some of these to work together
	-We will be working with Hadoop, installing and using some products
- Hadoop Distributions aim to resolve version incompatibilities


### HDFS
- A distributed file system modelled on the Google File System
- Data is split into blocks, typically 64MB or 128MB in size, spread across many nodes
- Each block is replicated to a number of nodes
	-ensures reliability and availability
- Files in HDFS are write once - no random writes to files allowed
- HDFS is optimised for large streaming reads of files - no random access to files allowed
- Storing large files
	• Terabytes, Petabytes, etc...
	• Millions rather than billions of files
	• 100MB or more per file (256MB)
	• Streaming data
	• Unstructured data => really mixed structured data
- Write once and read-many times patterns
- Optimized for streaming reads rather than random reads

#### When a client application wants to read a file
-  it communicates with the **NameNode** to determine which blocks make up the file,
and on which **DataNodes** the block reside
- it then communicates directly with the **DataNodes**

- NameNode is the single point of failure of a Hadoop system
	-backup periodically to remote NFS
	-use Secondary NameNode


### Files and Blocks
- Files are split into blocks (single unit of storage)
	• Managed by Name node, stored by Data node
	• Transparent to user
- Replicated across machines at load time
	• Same block is stored on multiple machines
	• Good for fault-tolerance and access (Can lead to inconsistent reads)
	• Default replication is 3