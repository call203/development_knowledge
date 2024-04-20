## Spark
- Spark is a **unified engine designed for large-scale distributed data processing**, on premises in data centers or in the cloud
- purpose processing engine that can be used **instead of MapReduce**
- **Spark SQL**
- **Spark Streaming** - stream processing of live data streams
- **MLlib** - machine learning - ]spark.mllib and spark.ml (for dataframes)
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
- **Resilient** : if the data is lost, it can be recreated for previous steps
- **Distributed** : appears as a single chunk of code, but is actually distributed across nodes
- **Dataset** : initial data can come from file or created programmatically
- An RDD is a read-only, partitioned collection of records
- supports in-memory processing computation
- RDDs represent data or transformations
- This is best suited for application that apply the same operation to all elements of a dataset
- Superseded by DataFrames
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