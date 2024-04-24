## Spark
- Spark is a **unified engine designed for large-scale distributed data processing**, on premises in data centers or in the cloud
- purpose processing engine that can be used **instead of MapReduce**
- **Spark SQL**
- **Spark Streaming** - stream processing of live data streams
- **MLlib** - machine learning - spark.mllib and spark.ml (for dataframes)
- **GraphX** - graph manipulation
- **extends Spark RDD** with Graph abstraction: a directed multigraph with properties attached to each vertex and edge.
- **Spark API** providing interface for programming clusters with implicit **data parallelism** and **fault tolerance.**
- Supports Java, Scala, Python and R

#### What is the MapReduce problems?
- Many problem aren't easily describe as map-reduce
- Persistence to disk typically slower that in-memory work
#### Main idea
- **Use the memory resources of the cluster** for better performance instead of writing to Disk
- Memory is faster than disk, but more **volatile** - **Fault tolerance** is built into framework

**Speed**
- **Directed Acyclic Graph (DAG)** and **Query Optimizer** => decompose into tasks that can be executed in parallel
- **Physical Execution Engine (Tungsten)**, generates and executes compact code

**Modularity**

**Ease of Use**
- **Abstraction using Logical data structure, Resilient Distributed Dataset (RDD)** -> upon which higher level abstractions such as Dataframes and Datasets are constructed
- **Transformations** and **Actions** (operations) are applied to these

**Extensibility** 
- Focused on computation engine - not on storage (Hadoop is storage and computation engine)
- Can use all storage engines - RDBMS, NoSQL, etc
	-Read data from these, use Spark to process this data (in-memory), and write results back to storage engine


### Spark Execution
Spark Program consists of . . . 

**Driver Program** 
 - responsible for orchestrating parallel operations on the Spark cluster
**Accesses the distributed components in cluster** 
- **Spark Executors** (runs each worker node in the cluster and executes the tasks) and **Cluster manager** (Managing and allocating resources on the nodes)
- through a **SparkSession**, Listens and communicates with distributed Executors.
**Spark Session** 
- entry point for all Spark functionality, parameters, data sets, queries, transformations, etc.
#### Spark Driver 
- Driver converts Spark into one or more jobs
- Each Job is transformed in a **DAG**
#### Spark Stage
- Stages are created **based on operations** that can be performed **Serially or in Parallel**
- Not all operations can happen in a single stage => divide into **multiple stages**
#### Spark Tasks
- **Each Stage** is comprised of **Task** - a unit of execution
- These are **distributed across** each Spark executor
- Each Task maps to a single core and works on a single partition of data
- Eg. If Executor has16 cores, can have 16 Tasks working on 16 partitions in parallel => makes things very fast


### Data Structures - Transformations & Actions
**Transformations**
- transform a Spark Data Frame from one format/structure to a different one
- Transformations **set things up, what do to**
- Define a new RDD based on the current one(s)
- Original Data is unchanged
- orderBy(), groupBy(), filter(), select(), join()

**Actions** 
- cause calculations/transformations to actually be performed
- Actions cause calculations to **actually** be performed
- can be applied to RDDs; actions force calculations and return values
- show(), take(), count(), collect(), save()

**Lazy evaluation**
- Nothing computed until an action requires it
- Allows for optimization of **distribution, execution, reorder** individual steps to reduce work/resources
- Result are not computed for each transformation
- **Lineage** is created – sequence of transformations
- Because Spark records each transformation in its lineage and the DataFrames are immutable between transformations, it can reproduce its original state by simply replaying the recorded lineage, giving it resiliency in the event of failures


### Resilient Distributed Dataset (RDD)  ******! important
A fault-tolerant collection of elements that can be operated on in parallel
- **Resilient** : if the data is lost, it can be recreated for previous steps
- **Distributed** : appears as a single chunk of code, but is actually distributed across nodes
- **Dataset** : initial data can come from file or created programmatically
- An RDD is a read-only, partitioned collection of records
- supports in-memory processing computation
- RDDs represent data or transformations
- This is best suited for application that apply the same operation to all elements of a dataset
- Superseded by DataFrames
- JVM Objects
- Distributed over a cluster of machines
- Immutable representation of data
- Operations (Transformations and Actions) on one RDD creates a new one
- Memory caching layer that stores data in a distributed, fault-tolerant cache
- Created by parallel transformations on data in stable storage
- Lazy materialization
- Transformations are lazy (not computed immediately) the transformed RDD gets
  recomputed when an action is run on it (default)
- RDDs are not suitable for everyone or for all data processing => Convert to a Dataframe
### Limitations of RDD
- There is no built-in optimization engine in RDD.
-  RDD cannot handle structured data.
-  Can be difficult to use, optimize, etc

### DataFrame
- Recommended to use instead of RDD
- Similar in manner to Python Pandas DataFrame. Similar
- **Benefits**
	-Suited to different types of Data (structured/unstructured)
	-Easier Data transfer (API) to/from Spark
	-In-built Optimized Execution Plan
	-Better Memory Management
	-Compatible with many Data Storage Engines (Databases)


### Spark SQL
- Spark SQL is a Spark module for structured data processing.
- It provides **DataFrames** and can also **act as a distributed SQL query engine** => simplifies use
- Allows for **Query Optimization**
- Scalability and Optimization (Lazy)
- Easy integration with external Data Sources
- Can combine Spark SQL with the other DSL
- Supports (a subset)of ANSI SQL
- Connect with other data software such as Tableau, Snowflake, and Power BI using JDBC/ODBC connectors


### Spark- Machine Learning 

*A lot of ML / AI / .... consists of defining/finding rules that exist in the data and business

#### Functionality 
-  Pre-processing your data (cleaning data and feature engineering)
- Supervised learning
- Recommendation learning
- Unsupervised engines
-  Graph analytics
- Deep learning
#### Typical Process
1. **Gathering and collecting** the relevant data for your task.
2. **Cleaning** and inspecting the data to better understand it
3. **Performing feature engineering** to allow the algorithm to leverage the data in a suitable form
4. Using a portion of this data as a training set to **train one** or more algorithms to generate some candidate models
5. **Evaluating and comparing models** against your success criteria by objectively measuring results on a subset of the same data that was not used for training. This allows you to better understand how your model may perform in the wild.
6. **Leveraging the insights** from the above process and/or using the model to make predictions, detect anomalies, or solve more general business challenges.

#### spark.mlib vs spark.ml
- MLib is general term used for machine learning Spark library
- **Spark.mllib** 
  the original ML API based on RDD API. Has been in maintenance mode since Spark 2
-  **Spark.ml**
   ML API based on dataframes – Recommended to use this going forward
   (Very similar/same as Python scikit-learn)


### Spark Advantages
- Continuous Delivery of Data = Current picture of Data
- **Streaming data** can be consumed from multiple sources — Kafka, Kinesis, Flume etc
- **Fault tolerant and fast** recovery from failures in case of any breakdown
- Better **load balancing** with optimized resource utilization
- Combining of live streaming data with **static datasets**
- **Interactive SQL** operations can be performed on live streaming data
- **Machine learning & Graph processing techniques** can be applied to live streaming data

### Spark Streaming
Spark Streaming introduced the idea of micro-batch stream processing, where the streaming computation is modelled as a continuous series of small, map/reduce-style batch processing jobs (hence, “micro-batches”) on small chunks of the stream data.

- Batches are called DStream (Discretized Streams)
- DStreams are represented as RDDs
- Similar to that of RDDs, transformations allow the data from the input DStream to be modified.
- DStreams support many of the transformations available on normal Spark RDD's
- Spark streaming provides **windowed computations**, which allow you to apply transformations over a sliding window of data.
	Every time the window slides over a source DStream, the source RDDs that fall within the window are combined and operated upon to produce the RDDs of the windowed DStream.