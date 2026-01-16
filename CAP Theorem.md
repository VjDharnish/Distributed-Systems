 CAP Theorem states that any distributed system or data store could simultaneously provide only 2 of the 3 guarantees
 - Consistency
 - Availability
 - Partition tolerance

### Consistency 
Consistency in distributed system states that regardless of the node they connect to , all the client should receive the same data response at once. In other words , Consistency refers to a database query returning the same data each time the same request is made. So for the write of one node to suceed, it must replicate to all the available nodes

### Availability
it states that even if one or more node become unreachable, the client must receive a vaild response  without exception. In this case, the  resuilts may outdated.

### Partition Tolerance
A partition is a break in communication - may be a node down or network failure. Parttition tolerance means that even in any breakage of the communicartion among the system, the cluster should work.

### Mandatory
A distributed system/datastore operates with network partition by  nature. So because of network failure is inevitable, **Partition Tolerance**  is neccesary for the reliable service. So the choice must be either Consistence or Availability. So the cost of higher availabilty is lower consistency and vice versa


### CP
The CP databases offer Consistency and Partition tolerance but not availabilty. The result is that , if any break in communication of nodes happen, the system become unavailable untill all nodes become reachable and healthy. Mongodb and redis are the examples of CP databases
### AP
The AP Databases offer Availabilty and Partition tolerance. As a result, in case of breakages, the system conitunes to run but may provide outdated results. ScyllaDB,Cassandra, Weaviate are examples.

### CA
CA database are only in theory. there are no practical. database which runs without partition tolerance.

Database systems such as RDBMS that are designed in part based on traditional ACID guarantees choose consistency over availability. In contrast, systems common in the NoSQL movement designed around the BASE philosophy select availability over consistency.

### Eventual Consistency
Eventual Consistency guarantees that when an update happens, that update will finally be reflected in all the nodes. In Eventual consistency, we may get outdated result at first at low latency. But the updated results will get received over time once it reaches all the replicas.

![Eventual Consistency](https://www.scylladb.com/wp-content/uploads//eventual-consistency-diagram-1.png)

Eventually consistent databases prioritize availability over strong consistency. Eventual consistency in microservices can support an always-available API that must be responsive, even if the query results may occasionally be missing the latest commit.


### FAQ
Why RDBMS dont have to be distributed?
The RBDMS are designed for ACID properties and easily manage workloads in a single node by nature . because it depends on complex joins and central locking mechanism to achieve consistency and availabilty, they perform poor in distributed system


[Ref](https://www.scylladb.com/glossary/cap-theorem/)
