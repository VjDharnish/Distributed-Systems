### ACID
-  #Atomicity 
	- The ability to abort the transaction if any error occurs in the middle and have all writes in the transaction discarded is the defining feature of atomicity

- #Consistency
	- Consistency is that we have certain state about the data that must be always true before and after an operation performed on that data (Invariant)
	- **While atomicity, isolation and durability are the properties of database, The consistency is the part of the application meanwhile the application must rely on atomicity and isolation of the database to achieve consistency.**
	
- #Isolation
	-  Concurrently running transaction should not interfere with each other. Ex, if one transaction made several writes, then another transaction should see either all or none of those writes
	- If multiple processes accessing the same part of the database, we can run into concurrency problem called race condition
	- Isolation in ACID means that concurrently executing transactions are isolated from each other. 
	- *serializability* -  which means that each transaction thinks that it is the only transaction in the entire database. 
	- The database ensures that when the transaction commited, the result is same as if they had run serially(one after another)
	- **Transaction don not inherently prevent all race conditon, we still need locking, abort-rety handling in the real world database
	
- #Durability
	- Durabiity is the promise that once a transaction is commited. It will not be forgotten even if any hardward fault or database crashes
	- But in cases like complete disk failure and it is not recoverable, there is nothing we can do.

#### Single object writes
Atomicity and isolation also apply to single object changes. 
 
 Imagine you are writing a 20 KB JSON document to a database:

	- If the network connection is interrupted after the first 10 KB have been sent, does the database store that unparseable 10 KB fragment of JSON?
    
	- If the power fails while the database is in the middle of overwriting the previous value on disk, do you end up with the old and new values spliced together?
    
	- If another client reads that document while the write is in progress, will it see a partially updated value?

So storage engines almost universally aim to provide atomicity and isolation on single object level. *Atomicity can be implemented using a log for crash recovery* and *isolation can be implemented using a lock on each object*

