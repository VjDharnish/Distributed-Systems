 Quorum read/write is a distributed system technique for maintaining consistency and availability , requiring a minimum number of replica to acknowledge a operation.

### Consistency rule
W + R > N , where W = writes, R = Reads, N = Number of replicas
this rule ensure that the reads guaranteed as latest write  to any client.

### Majority Quorum
A common configurations where W and R are set to  (N/2)+1 for both speed and fault tolerance.