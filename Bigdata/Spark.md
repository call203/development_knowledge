## Spark
- designed for large-scale distributed data processing, on premises in data centers or in the cloud
- general-purpose processing engine that can be used instead of MapReduce
- Java, Python, Scala

### Main idea
use the memory resources of the cluster for better performance
#### Speed
**Directed Acyclic Graph (DAG)** and Query Optimizer => decompose into tasks that can be executed in parallel

#### Ease of Use
- Abstraction using Logical data structure, **Resilient Distributed Dataset(RDD)**
- Dataframes and Datasets are constructed
#### Modularity
#### Extensibility 
- Can use all storages engines - RDMS, NoSQL, etc
- Focused on computation engine -not on storage




#### Spark Execution
##### Driver Program 
 - responsible for orchestrating parallel operations on the Spark cluster
 - Driver converts Spark application into one or more jobs
 - Each job is transformed in a **DAG**

##### Spark Stage
- Stages are created based on operations that can be performed Serially or in Parallel
- divided into multiple stages

##### Spark Tasks
- Each Stage is comprised of Task - a unit of execution
- These are distributed across each Spark executor
- Each Task maps to a single core and works on a single partition of data



#### Data Structures - Transformations & Actions !! important
##### Transformations
- transform a Spark Data Frame from one format/structure to a different one
- Original Data is unchanged

##### Actions 
- cause calculations/transformations to actually be performed

##### Lazy evaluation
- Nothing computed until an action requires it


#### Resilient Distributed Dataset (RDD) !! important
- Resilient : if the data is lost, it can be recreated for previous steps
- Distributed : appears as a single chunk of code, but is actually distributed across nodes
- Dataset : initial data can come from file or created programmatically

- An RDD is a read-only, partitioned collection of records
- RDDs represent data or transformations(new RDD based on the current one) on data
- Actions can be applied to RDDs; actions force calculations and return values
- Superseded by DataFrames


#### DataFrame  !! important
- Recommended to use instead of RDD 
- Similiar in manner to Python Pandas DataFrame
