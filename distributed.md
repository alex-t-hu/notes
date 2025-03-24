### Mapreduce
map (k1, v1) -> list(k2, v2)
reduce (k2, list(v2)) -> list(v2)

### The design of a practical system for fault-tolerant virtual machines
- perfect replication (machine state, instead of application state)
- output problem (be careful to synchronize fully when writing outputs)
- bounce buffer intermediate buffer for data transfer (high-to-low, I/O, DMA)
- split brain avoided by atomic test and set on shared virtual disk storage when going live (can't have 2 machines go live at once)
- output rule - primary can't send output to the external world until backup has received and ackwed the log with the entry producing output
- lag problem - be sure to lag the primary by adjusting CPU if it is too far ahead

### Consistency
- strong consistency (Every read returns the most recent write)
	- linearizable consistency
	- sequential consistency
- weak consistency
	- client-centric consistency (A client always sees its own updates, but other clients might see stale data)
	- causal consistency (Reads respect causal dependencies but not necessarily real-time order)
	- eventual consistency (Reads may return stale values, but will eventually converge to the latest value)

- primary/backup replication is strongly consistent
	- asynchronous ("update") vs synchronous ("update" + "ack")
	- if primary fails, the secondary backup could be behind or ahead
- 2PC is strongly consistent
	- coordinator asks nodes if ok to commit. coordinator then asks nodes to commit/rollback
	- doesn't work for partitions
- distributed consensus is strongly consistent and partition-tolerant
## Raft
- **Election safety** - at most one leader can be elected in a given term
- **Leader Append-Only** - a leader never overwrites or deletes entries in its log; it only appends new entries
- **Log Matching** - if two logs contain an entry with the same index and term, then the logs are identical in all entries up through the given index
- **Leader Completeness** - if a log entry is committed in some term, then it will be present in the logs of all leaders in higher-numbered terms
- **State Machine Safety** - if a server has applied a log entry at a given index to its state machine, no other server will apply a different log entry for the same index

- Post election timeout, follower increment current term, transition to candidate state, vote for itself, issue RequestVote RPCs
	- votes only allowed to 
	- wins election. sends heartbeats to establish authority and prevent new elections.
	- another server becomes leader.
	- period of time goes by with no winner.

- AppendEntries RPCs
	- only sent by leaders
- RequestVote RPCs
	- only sent by candidates

If client crashes in the lab, would the response be sent?
Difference between real life and lab - we need to disable sendHeartbeats when something is killed ... 
### GFS
- designed for large streaming reads and small random reads, large sequential writes that append data to files
- interface - create, delete, open, close, read, write, snapshot, record append
- the master polls chunkserver for which chunkservers have a given replica at startup. the operation log contains a record of metadata, like files and chunks and their versions, and is replicated over multiple remote machines. 
- a file region is consistent if all clients see same data, defined if it is consistent and client sees all mutation writes
- the primary chunkserver refreshes once in a while. it has a "chunk lease". writes go thru the primary to maintain a consistent order
- every time a file is re-opened, the cached chunkserver map is returned. otherwise the chunkserver map could be stale
- stale replicas are caused by chunkservers failing, missing mutations. there's a chunk version number to help identify stale replicas
- chunkservers store checksums durably and use it to confirm their info is correct; requestors might need to read from other replicas
- append record (append without specifying exact offset, returns offset where record was appended) can actually duplicate parts of files, and each replica can have inconsistent offsets due to failures, retries

### Zookeeper
- high performance distributed "filesystem" (files are bounded in size, in-memory), faster in read-dominant
- it's called "wait free" because different clients don't have to wait on each other
- designed for coordination data (status info, config, location, data / node is small, byte to kilobyte range)
- fencing occurs when a client's session expires for whatever reason, so zookeeper ignores the clients requests
- clients can set a watch on znodes
- create (create a node), delete, exists (test if node exists at a location), get data, set data, get children, sync
- namespace like a filesystem. each node can have data + children (unlike a directory)
- znodes are held in a session and they can be ephemeral or not (deleted after client disconnects)
- write request forwarded to leader server, which serializes it, assigns a Zxid, atomic broadcast only commits until majority of followers accept. commit writes to disk for durability. can batch multiple concurrent writes, no guarantee about interleaving.
- A-linearizability for writes - all requests that update the state are serializable and respect precedence. FIFO client order - all requests from a given client are executed in order sent by client

### Chain Replication with Apportioned Queries
less fault-tolerant than raft and zookeeper. chain of nodes, write to head propagates all the way to tail, where it is then committed. if we're in the middle of committing some write, that intermediate node can still return that write's value, it just needs to confirm what is committed or not with the tail. since the confirmation is less expensive then sending the whole data, the burden on the tail is less.
### Locking
- simple locking locks before reading/writing
- two-phase locking 
	- growing phase (obtain locks), shrinking phase (release locks)

### Operational Transform
if u have one user adding a char and the other deleting, when exchanging operations, operations need to be transformed
### CRDT
Applications can update any replica independently, concurrently, and without coordination. The algorithm automatically resolves inconsistencies. The state eventually converges.
grow-only counter
yjs/automerge are libraries

### Go
Use the race detector, for development and even production. Convert data state into code state when it makes programs clearer. Convert mutexes into goroutines when it makes programs clearer. Use additional goroutines to hold additional code state. Use goroutines to let independent concerns run independently. Consider the effect of slow goroutines. Know why and when each communication will proceed. Know why and when each goroutine will exit. Type Ctrl-\ to kill a program and dump all its goroutine stacks. Use the HTTP server’s /debug/pprof/goroutine to inspect live goroutine stacks. Use a buffered channel as a concurrent blocking queue. Think carefully before introducing unbounded queuing. Close a channel to signal that no more values will be sent. Stop timers you don’t need. Prefer defer for unlocking mutexes. Use a mutex if that is the clearest way to write the code. Use a goto if that is the clearest way to write the code. Use goroutines, channels, and mutexes together if that is the clearest way to write the code.

### Hadoop Distributed Filesystem

### Transactions
2-phase locking
- useful in non-distributed and distributed
- acquire locks before using, release when commit, to enforce serializability
- find deadlocks with wait-for graph
2-phase commit
- enforce atomicity of distributed transactions (either all nodes commit or none do)
- prepare phase (write transaction to log on disk, respond yes/no), commit phase (send commits to everyone if everyone said yes, otherwise send abort )
- if coordinator fails, participants block after prepare phase until receive commit message. so that's why coordinator is replicated
- 3 phase commit hard, can't distinguish between network failures

cross-machine atomic ops - transfer(x, y) involves add(x, 1), add(y, -1) atomically
transactions (can be distributed or not)
- begin, add(x, -1), add(y,  1), commit
- begin, get(x), get(y), print t1, t2, commit
- abort
- ACID - atomic, consistent, isolated, durable
- isolation: serializability: T1 || T2 = T1; T2 or T2; T1. doesn't have to respect time, so weaker than linearizability. used in the database community, refers to transactions vs. linearizability refers to simple reads/writes.
- concurrency control - pessimistic (locks), optimistic (no locks, better if transactions conflict yes)


### Spanner
R/W  transaction: 2PC + 2PL + paxos groups (inefficient)
- reads go directly to all the participants (shards), writes go to transaction coordinator, participants are all paxos groups
R/O transaction: snapshot isolation, synchronized clocks
- read from local shards, no lock, no 2PC
- every transaction has a timestamp, when it started. timestamps are intervals
- start rule - now.latest so that the time assigned as after true time
- commit wait: delay a commit until now.earliest > d
correctness - serializable, external consistency (t < s, s sees t's writes)