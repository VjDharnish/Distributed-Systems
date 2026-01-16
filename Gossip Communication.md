### Concept
- Nodes randomly exchange information with other nodes.
- Can be used for various forms of information dissemination, including data sharing, state synchronization and **membership management**
### Characteristics
- **Decentrailzed** -  No single point of failure
- **Eventual consistency** - Information take some time to propagate
- **Local Information** - Nodes only need local information to interact with others

## SWIM Protocol (Scalable Weakly-consistent Infection-style Process Group Membership)
- An implementation of gossip protocol to discover and monitor cluster nodes.
- Extremely light weight
- its speed and load per node do not depend on cluster size
	- Means Each node's performance remains constant, regradless of how many nodes add or remove from the cluster.
- This protocol solves Several tasks at once
	- First - Build and keep up to data topology of a cluster without explicit configurations
		- When new node started , it doesnot know anything about the cluster, and already running nodes can fail. 
		- According to the protocol, cluster nodes broadcast packets to discover new nodes and send p2p ping request to detect failures
	- Second - Event dissemeniation in the cluster.
		 - Event can be node fallure, uuid change, ip adrress update, new node appearnce etc - anything that affects cluster state
### Guarantees the following
- Constant time taken to learn about an event or a new node node in the cluster
-  Logarthmic from the cluster size time to disseminate the event to each node of the cluster