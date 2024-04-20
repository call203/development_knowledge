# Hadoop

### **What is Big Data**
- Big data is when the size of the data itself becomes part of the problem
- Not every company will benefit from Big Data, it depends on your size and your ability
- Everything we do is increasingly leaving a digital trace which we can use and analyse


### **Hadoop**
- programming models to process large data sets
- be written in Java
- built on Hadoop clusters, which are collections of computers or notes, that work together to execute computations on data


##### Principles
- Scale-Out rather than Scale-Up
- Bring code to data rather than data to code
- Abstract complexity of distributed and concurrent application
- Self managing
- Auto parallel processing

![Screenshot 2024-02-08 at 12 58 43](https://github.com/call203/note_taking/assets/57412714/90bfb5cb-7148-495c-9229-ba6809218681)







##### RDBMS vs Hadoop
![Screenshot 2024-02-08 at 12 59 22](https://github.com/call203/note_taking/assets/57412714/38d476fc-1e5c-409f-a347-f241cfbf190e)



##### Hadoop Ecosystems
![Screenshot 2024-02-01 at 11 53 18](https://github.com/call203/note_taking/assets/57412714/b86eb8f1-4814-4761-b1fd-afbb8e6b3843)



- MapReduce
- Hadoop Distributed File System (HDFS)
- Yet Another Resource Negotiator (YARN)




#### MapReduce
A framework fro processing parallelizable problems across large datasets using a large number of computers(nodes), collectively referred to as a cluster
** On Peta Bytes of data!
- we can use Map and Reduce
- With Map, we can convert a set of data into tuples (key/value pairs)
- Reduce takes the output of Map as input and combines the tuples into smaller sets of tuples
- make it easy to scale data processing to run tens of thousands of machines in a cluster
  
![Screenshot 2024-02-08 at 13 01 06](https://github.com/call203/note_taking/assets/57412714/78ba4dfe-0546-447a-a969-c4696c390c69)




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


##### Jobtraker
 - Controls MapReduce jobs
 - Assigns Map & Reduce tasks to the other nodes on the bluster
 - Monitors the tasks as they are running
 - Relaunches failed tasks on other nodes in the cluster

##### Task Trackers
- A single TaskTracker per salve node
- Manage the execution of the individual tasks on the node
- Can instantiate many JVMs to handle tasks in parallel
- Communicates back to the JobTracker
  
![Screenshot 2024-02-08 at 13 04 20](https://github.com/call203/note_taking/assets/57412714/0c2a2a2c-5e12-4735-a6e9-71176ed49a8d)
