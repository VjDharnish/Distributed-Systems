## Types of Partitioning
- partitioning by key range -  skewness happens if large number of writes to specific key range
- Hash based partitioning 
	- skewness happens if large number of writes happen to a singlekey
	- Can be handled in application layer as  prefixing/suffixing 2 decimals to the key at write store the data to various partitions. For read, we have to request to all partitions and then combine which requires additional bookkeeping for keys.
	- Note- apply this case for hotkeys (keys with high writes) 
	- If applied for all/majority of keys could be a unnecessary overhead cause low write throughput

#### Secondary index and its partitioning
 - Document based partitioning - local index, should query for in all partitions and then combine
 - Term based partitioning - A global index but should be partitioned to all nodes.

## Rebalancing partitions

### Minimum Requirements should be satisfied for rebalancing
- The load should be distributed fairly across nodes
- While rebalancing , the database should be able to accept read and write
- no more data than necessary ones should be moved between nodes, to make rebalancing fast and minimize Network and Disk I/O

### Strategies for rebalancing
#### What not to do  -> Hash Mod N
- While adding more nodes , we have to move data more frequently than necessary, this makes rebalancing costly.
- Example - if hash(key) = 123456 , for 10  nodes , the should move to part6 (123456%10) , when growing for 11, it should be 3, for 12 it should be 0, etc..

#### Fixed number of partitions
 - A database running on 10 node cluster, created with 1000 partitions can be fairly distributed like 100 partition per node approx.
Pros
 - When adding nodes, only some partitions from some/all nodes can be moved to new nodes
 - We can move partition from less powerful to more powerful hardware nodes.
 Cons
- We can't set fixed partition, if dataset is highly variable
- If size of each partition grows very large, rebalancing and node recovery becomes expensive. 
- if partitions are too small, they cause too much overhead

#### Dynamic partition
- For key based partioning, range for each partition can be change with respect to the amount of data present. Some Databases such as HBase,RethinkDB create partitions dynamically.
- An advantage of dynamic partitioning is that the number of partitions adapts to the total data volume. If there is only a small amount of data, a small number of partitions is sufficient, so overheads are small; if there is a huge amount of data, the size of each individual partition is limited to a configurable maximum
- MongoDB supports both key based and hash based partitioning, it splits up partitions dynamically ion either cases.

#### Parititioning proportional to Nodes
- With dynamic partitioning, no of partitions proportional to the volume of data an size of each partition is proportional to size of the data 
- In both cases, the partitions are independent of no pf nodes.
- Cassandra provides 3rd option is to make no of partitions proportional to the number of nodes
In other words, fixed no of partitons per node.
- When new node joins the cluster, equal no of partitions created and existing partition data are splitted and moved to the new partitions
- The size of each partition grows proportionally to the dataset size while the number of nodes remains unchanged, but when you increase the number of nodes, the partitions become smaller again. Since a larger data volume generally requires a larger number of nodes to store, this approach also keeps the size of each partition fairly stable.

### Request Routing Approaches
- Send request randomly to a server, the server handle itself or redirect to the partition owner . Example Cassandra and Weaviate
- Send request to a routing tier which maintains meta of the nodes and route to the respective node . Note routing ties doesnot handle any requests
- Client need to be aware of the partitions

