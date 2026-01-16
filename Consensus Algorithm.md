The complex distributed system works independently to achieve a common goal; they have to agree on common decisions. For example, which transactions to commit to the database in which order. So, the system should function continuously in case of failures in nodes.

This requires the consensus mechanism we choose to be fault-tolerance or resilient. Hence, the choice of opting consensus algorithm depends on the problem we need to tolerate.

Must have properties of consensus algorithms,

1. Termination - All running processes must decide on a value.
    
2. Agreement - All running processes must decide the same value.
    
3. Integrity - If all running processes decide a certain value, then any running process must accept the value.

## Paxos
Leaderless
3 phases - Prepare, Promise, Accept
Involves message exchange between Proposer and acceptor

Prepare 
	Proposer send a message with proposal number to acceptor.
	If proposal number greater than previous proposal number, the acceptor seen, it replies as it accept the proposal and don't accept any with lower proposal number as promise

Promise
	The promise from the acceptor includes acceptance status and highest proposal number it previously accepted(if any)

Accept
	Once proposer receives more number of promises from acceptors(QUORUM) it moves on to the acceptance phase
	Proposer then send a accept message with the proposal number and value to be accepted to the acceptors
	If the proposal number is valid, the acceptor accepts the proposal and informs the proposer

Learner :
	The learn phase ensures that the decision is communicated to all the nodes in the system by a dedicated learner node, so that the system achieves agreement on the chosen value.
	Alternatively the proposer can explicitly send learn details to other nodes after a value is accepted.	



## Raft
Leader based model 
A node can be a leader, follower or candidate
2 Phases - Leader Election, Log Replication.

### Leader Election
- In raft there are two timeout settings which controls the elections
- Each node has a **election timeout period** , once this time out and didn't get heartbeat from leader, it becomes candidate and starts the leader election process and vote for itself.
	(The election time  between 150ms - 300ms)
- The candidate sends a request for vote to other nodes. if a node has not voted in the current term and it has not a leader already, it votes for the candidate.
- The candidate become leader when majority of the node accepts(quorum).
- The leader now sends append entries messages to the followers
- The messages has been sent in intervals specified by the **heartbeat timeout**
- The election term is managed and only one leader per term based on majority of votes.
- If multiple nodes becomes candidate, then vote split should happen and nodes only vote for one candidate(First come) . 
- if all candidates receive equal number of votes, then new election happens in next term after the election timeout of the nodes. 

### Log Replication
- The client send data only to the leader
- Once the leader receive value from client, it append the data to its local log and it remains uncommitted.
- The leader then send the log to the followers and waits untill majority of the followers written the entry to their log.
- The leader notifies the followers to commit the changes. 


### Raft stay consistent even in the network partition
- We can end with number of leaders = number of partitions happened
- Here multiple clients can send data to multiple leaders
- The leader without enough majority with keep the changes(log entry) uncommited. And the leader with majojity will succeed the transaction commited.	
- once the partition removes, Only leader available based on the highest term and other leaders should step downthe. All the nodes get updated with the last commited entry of the leader. 
	
	
