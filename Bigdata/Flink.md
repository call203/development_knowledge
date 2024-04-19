# Flink
---------- 

Apache Flink is an open-source, **unified stream-processing and batch-processing framework** developed by the Apache Software Foundation.
( written in Java and Scala)

- Flink does not provide its own data-storage system, but provides data-source and **sink connectors** to other database systems.
- For **Batch** and **Real-Time** data processing
- automatically compiled and optimized into dataflow programs that are executed in a cluster or cloud environment.
- a high-throughput, low-latency streaming engine as well as support for **event-time processing**. It is also fault-tolerant.
- Everything is a stream, it's just some stream are **bounded**, whilst others are **unbounded**.
- distributed processing engine for stateful computations over unbounded and bounded data streams.
- Programs can be written in Java, Scala, Python and SQL


### Key Concepts
#### **Stream Processing**
  - Analysing the data can be organised around **bounded and unbounded streams** of data.
  - **Batch processing** is the paradigm at work when you process a bounded data stream.
  - **Unbounded streams**: the input may never end, and so you are forced to continuously process the data as it arrives.
  - Application are composed of **streaming dataflows** that may be transformed by user-defined **operators**. These datablows form directed graphs that start with one or more sources, and end in one more sinks (a component or operation in the pipline)
  
#### Parallel Dataflows
- During execution, a stream has **one or more stream partitions**, and each operator has **one or more operator subtasks** 
- **Subtask** execute in different threads and possibly on different machine or containers.
- **The number of operator subtasks is the parallelism** of that particular operator

#### Stateful Stream Processing
- Flink’s operations can be stateful.
- how one event is handled depends on the accumulated effect of all the events before it.
- The set of parallel instances of a stateful operator is effectively **a sharded key-value** store. Each parallel instance is responsible for handling events for a **specific group of keys**, and the state for those keys is kept locally.

#### Fault Tolerance using State Snapshots
- By a combination of **state snapshots** and stream replay.
- These snapshots capture the **entire state of the distributed pipeline** asynchronously
- When a failure occurs, the sources are rewound, the state is restored, and processing is resumed.

#### Stack/Layers of Flink

APIs& Libraries : DataStream API (Stream Processing) | DataSet API(Batch Processing)
Runtime : Runtime (Distributed Streaming Dataflow)
STORAGE : Local( Single JVM ) | Cluster (Standalone, TARN) | Cloud (GCE, EC2)

- The **runtime layer** receives a program in the form of a J**obGraph**. A JobGraph is a generic parallel data flow with arbitrary tasks that consume and produce data streams.
- The JobGraph is executed according to a variety of deployment options
- Both the **DataStream API** and **the DataSet API** generate JobGraphs through separate compilation processes. The DataSet API uses an optimizer to determine the optimal plan for the program, while the DataStream API uses a stream
- Libraries and APIs are table for queries on logical tables, FlinkML for Machine Learning, and Gelly for graph processing.

### Architecture 
- **Program** 
  It is a piece of code, which you run on the Flink Cluster.
- **Client**
  It is responsible for taking code (program) and constructing job dataflow graph, then passing it to JobManager. It also retrieves the Job results.
- **JobManager**
  After receiving the Job Dataflow Graph from Client, it is responsible for creating the execution graph. It assigns the job to TaskManagers in the cluster and supervises the execution of the job.
- **TaskManager**
  It is responsible for executing all the tasks that have been assigned by JobManager. All the TaskManagers run the tasks in their separate slots in specified parallelism. It is responsible to send the status of the tasks to JobManager.

### DataStream - Core Components 
1. **Configuration & Connection** 
	- Obtain an execution environment and  the initial data
	- Specify transformations on this data,
	- Specify where to put the results of your computations,
	- Trigger the program execution

2. **Connectors**
   **Data Source Connector**
   - Specify where the data comes from 
   - reading from files, directories, and sockets, and ingesting data from collections and iterators
	**Data Sink Connector**
   - sending the processed or transformed data out of the data processing pipline.
   - write the data to different destinations
3. **DataStream**
4. **Tranformations**
   Transformations are operations applied to DataStreams to process, filter, aggregate, or otherwise manipulate the data.
5. **Sink**
   - Sinks are the endpoint in a Flink program where the output from a DataStream is written. Sinks specify where the processed data will be sent after transformations are applied. This could be writing data to files, databases, or other systems.
6. **Connectors** (again)


### Table API & SQL
- Apache Flink features two relational APIs - **the Table API and SQL** - for unified stream and batch processing
- Allows the composition of queries from relational operators such as selection, filter, and join in a very intuitive way
- Uses Apache Calcite
- The Table API and SQL are integrated in **a joint API**. The central concept of this API is a Table which serves **as input** and **output of queries**

### Table API - Apache Calcite
- Apache Calcite is a foundational software framework that provides **query processing**, **optimization,** and **query language** support
- It does now look after the storing and processing data
- **Query Optimise**r - Calcite optimizer applies optimization on the relational expression
	include: 
	-Predicate Pushdown: Pushing filters as close to the data source as possible to reduce the amount of data fetched
	-Join Reordering: Rearranging join order to minimize intermediate results and improve efficiency
	-Projection Pushdown: Pushing down projections to avoid unnecessary columns from being processed
	-Index Usage: Identifying and utilizing indexes to speed up data retrieval

### Table API - SQL
- **Define Table** 
  -On exsiting data using connector
  -Create a new one either temporarily or permanently : permanent Table sneed a **Catalog** to maintain the Meta-data such as databases, tables, views ... Additionally, functions and information needed to access data stored in database or other external systems
- **2 ways to query the data**
	-Strucutured
	-SQL
- **Output**
	-Table

### References
- https://freedium.cfd/https://javier-ramos.medium.com/flink-in-a-nutshell-b32eea2c3f20
- https://nexocode.com/blog/posts/what-is-apache-flink/
- https://calcite.apache.org/
