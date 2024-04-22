# MapReduce

A batch based, **distributed computing** framework for **processing parallelizable problems across large dataset**s using a large number of computers(nodes), collectively referred to as a cluster
- you can process any data
- This is good for handling peta bytes of data
- reduce tasks which are scheduled for remote execution on slave nodes
- it works by manipulating key/value pairs in the general format
```
map(key1,value1)➝ list(key2,value2)
reduce(key2,list(value2)) ➝ (key3, value3)
```

**Job** : a full programme
**Task** : the execution of a single map or reduce task over a slice of data called a split
**Mapper** : a map task
**Reducer** : a reduce task


### Map
- The Mapper takes as input a key/value pair which represents a logical record from the input data source
- it produces zero or more outputs key/value pairs for each input pair
	- a filtering function may only produce output if a certain condition is met
	- counting function may product multiple key/value pairs, one per element being counted
- The Mapper may use or ignore the input key
- Key = byte offset into the file where the line starts (can be considered irrelevant)
- Value = contents of the line in the file

### Reduce
- A single Reducer handles all the map output for a unique map output key
- A reducer outputs zero to many key/value pairs
- The output is written to HDFS files, to external DBs, or to any data sink
- it aggregates the data

### Job Tracker ( Master )
 - Controls MapReduce jobs
 - Assigns Map & Reduce tasks to the other nodes on the bluster
 - Monitors the tasks as they are running
 - Relaunches failed tasks on other nodes in the cluster

### Task Trackers ( Slave )
- A single TaskTracker per salve node
- Manage the execution of the individual tasks on the node
- Can instantiate many JVMs to handle tasks in parallel
- Communicates back to the JobTracker using heartbeat (to indicate still alive and whether they are ready for a task)

### Shuffle
- Integrates the data(key/value pairs) from outputs of each mapper

### Sort
- The set of intermediate keys on a single node is automatically sorted by Hadoop before they are presented to the Reducer
- Sorted within key




### Data Locality 
This is a local node for local Data
- Whenever possible Hadoop will attempt to ensure that a Mapper on a node is working on a block of data stored locally on that node vis HDFS
- If this is not possible, the Mapper will have to transfer the data across the network as it accesses the data – this is something we want to avoid
- Once **all the Map tasks are finished**, the map output data is transferred across the network to the Reducers
- Although Reducers may run on the same node (physical machine) as the Mappers there is no concept of data locality for Reducers

### Bottlenecks
- **Reducers cannot start** until **all Mappers** are **finished** and the **output has been transferred** to the Reducers and sorted
- To alleviate bottlenecks in Shuffle & Sort 
  **The percentage of Mappers** which should finish before the Reducers start retrieving data is **configurable**
- To alleviate bottlenecks caused by slow Mappers -Hadoop uses **speculative execution**
  -If a Mapper appears to be running significantly **slowe**r than the others, a **new instance of the Mappe**r will be started on another machine, operating on the same data (remember replication)
  -The results of the **first Mapper to finish** will be used
  -The Mapper which is still running **will be terminated** by Hadoop


#### MAP TASKS
##### Input Splits
divide into fixed-size pieces called input splits. Input split is a chunk of the input that is consumed by single map

##### Mapping
- data in each split is passed to a mapping function to produce output values
- a job of mapping phase is to count a number of occurrences of each word from input split and prepare a list in the form of <word, frequency>


#### REDUCE TASKS
##### Shuffling
- consumes the output of Mapping phase.
- Its task is to consolidate the relevant records from Mapping phase output.
- the same words are clubed together along with their respective frequency.

##### Reducing
- combines values from Shuffling phase and returns a single output value
- aggregates the values from Shuffling phase (calculates total occurrences of each word)
