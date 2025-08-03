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
- 2PC is strongly consistent + guarantees atomicity
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

### No compromises: distributed transactions with consistency, availability, and performance
- FaRM (fast remote memory) uses nvram (non-volatile ram is backed by batteries so it's persistent) and rdma (1-sided rdma reads/writes cache line with kernel bypass). optimistic concurrency control. can be 100x faster than spanner.
- Setup - configuration manager, with zookeeper, data sharded with primary/backup replication
- Execute phase (read data and version number with RDMA and do stuff)
- Commit phase
	- lock (TC sends to primary of each written object the new value and old version number, and each primary locks object and checks version number using atomic operations)
	- validate (TC sends to primary of each object's version # and lock flag, ). then, makes decision to commit.
	- commit backup first because in case primary crashes backups have data
	- commit primary (returns to TC earlier than the data is written by primary since using RDMA)
	- truncate (remove not needed log entries, sent to both primary and backup)

### Grove: a Separation-Logic Library for Verifying Distributed Systems
- separate config server, replicated with paxos, to handle leadership changes
- any replica can serve linearizable reads with leases that guarantee configuration doesn't changes. replicas wait for relevant write commits to happen
- durable log
- read throughput decreases with more servers; write throughput increases

### Chardonnay
- reaction to Spanner, spanner r/w transactions are slow -- 14ms and relies on fancy time synchronization
- transactional key-value store for single-datacenter deployments aiming for fast and general-purpose transactions on disk-based storage
- Fast two-phase commit, 150 microsecond on Azure with fast RPCs, low-latency storage for transaction logs, and careful protocol design
- key universe partitioned into disjoint subsets called ranges, each of which has 3 servers and a WAL replicated with Paxos placed on NVMe, and database on SSD. leader holds a lease and maintains a lock table to implement two-phase locking; all reads and writes go through leader
- supposedly the storage is very fast using "NVMe" which refers to PCI express bus, maybe 10 microseconds read vs SSD read of 100 microseconds
- epoch interval is around 10 ms to balance data being less stale with Epoch service being overloaded since all transactions talk to it, and this forces chardonnay to use 1 datacenter
- the r/o transactions are somewhat slow to make r/w faster, and dry run hurts low contention transactions
- read only transactions don't grab locks since they grab the current epoch (increased monotonically by epoch manager) and grab key values before the current epoch. it waits for locks if necessary
- non-stale read only transactions wait until the next epoch is past, similar to commit wait in spanner
- the transaction state store stores active transactions in a fault-tolerant, replicated way to mitigate 2PC blocking
- dry-run execution to identify read and write sets, prefetch read data into memory without locks just like a read only transaction. aim to reduce 2PC lock contention, because read records from disk is the bottleneck. also reduces deadlock.
- may need to wound wait, which means kill later transactions holding locks to avoid deadlock, even with the dry run, since the reads might have changed
- lock-free strongly consistent snapshot reads with epoch-based versioning system
- deadlock avoidance by knowing read and write sets in advance
- eRPC is an efficient RPC that uses kernel bypass to be 2 microseconds instead of 100 microseconds 

### Memcached at facebook
- store number of likes. read efficiency is important, write efficiency less important. reads are not linearizable, eventual consistency; writes are ACID transactions.
- multiple mc clusters within each region, each cluster replicates the cached data, multiple replicas for more efficiency
- regional pool shared by all clusters for less popular items
- the thundering herd problem is when client updates DB and deletes a key, and all clients get() and fetch from DB but miss. Solved by mc gives the first missing client a lease, mc tells others to trie get() again in a few miliseconds
- bringing up a new mc cluster could be perf problem with 0% hit rate, so clients of new cluster first get() from existing cluster and put() into new cluster()
- writes go directly to primary DB with transactions, and primary DBs distribute logs to secondary DBs that are applied
- example race: k not in cache, C1 get(k), misses, reads v1 from DB; C2 writes k and then deletes k; C1 then finally puts v1 into k. solved by leases; mc gives C1 a lease on k with the miss, permission to write k, C2's delete k invalidates C1's lease, so mc ignores C1's put k
- subtlety - we could have in theory DB sends data directly to mc, but DB doesn't know how to compute values for mc, since generally client code computes them from DB results

### Amazon DynamoDB
- fully managed, multi-tenant (multiple users stored same computer), predictable latency at any scale, non-relational, optimistic concurrency control
- Recovery manager scans ledger to look for stalled transactions and assign a new transaction coordinator. Each transaction coordinator maintains a ledger. 
- Single request vs. Multi request transactions
	- Single-request
		- TransactGetItems() is 2PC and includes a check on when items were last written to to ensure serializability
		- TransactWriteItems() - instead of updating read timestamps, to avoid the cost of writes, 
			- All reads reflect a single, serializable snapshot. First read - get value and reject if anyone is being written to, also return committed log sequence number. Second phase - returns item's value and also committed log sequence number (LSN). If changed, reject. Otherwise, return value.
		- predictable latency, no distributed locking, simple recovery
	- Traditional RDBMS - multi-request
		- one transaction type - intersperse reads and writes
		- unpredictable latency, locking nightmares, recovery complexity
- Operations
	- Not atomic (query, scan, batch (get, put, update, delete))
	- Atomic
		- multi-statement transactions (explicitly atomic)
			- mutating (write)
			- non-mutating (read)
	- single item (implicitly atomic, bypass 2PC)
		- getitem, putitem, updateitem, deleteitem
		- writes are sometimes serialized before or after transactions depending on if transactions have dependencies on the writes
		- can use ConditionExpressions to figure out whether to write or not
- Storage
	- stores one version (latest), 3 identical copies for fault tolerance, no multi-version-concurrency-control (MVCC)
	- partitioned based on hash.
- Performance & scaling
	- single-digit ms latency, millions of requests per second, scales horizontally by adding more partitions, load balancing
- Consistency - moving a database from one valid state to another
	- https://jepsen.io/consistency/models
	- tombstone mechanism - a marker left behind in place of deleted item. if a stale replica tries to resurrect the deleted item with a late write, any write older than the tombstone timestamp is ignored. tombstones kept for a configurable period called GC grace period. during compaction, the tombstone and data are purged if all replicas have seen tombstone, no write with earlier timestamp can appear.
	- alternative, store a partition-level max delete timestamp, and reject writes that 
- Isolation - keeping concurrent transactions (operations) apart

### On-demand Container Loading in AWS Lambda
erasure coding - data broken up into chunks and redundant chunks added with say reed-solomon codes. this improves tail latency when loading from replicated caches instead of 1 cache.
sparse loading - load only chunks that are needed
lifecycle of data chunks - active (read and write), retired (read only), expired (alarm on success), deleted
virtio - standardized interface for efficient I/O virtualization. they used block-level virtio-blk interface btwn MicroVM guest and hypervisor
overlayfs - union mount filesystem implementation for Linux
deduplication - hash chunks and lots of shared chunks
convergent encryption ensures identical plaintext inputs produce identical ciphertext outputs (unlike normal encryption that uses a salt), enabling deduplication while preserving security

### Ray
- Distributed futures, with ray.remote, execute on some other computer and return references to the object, so expensive copies in btwn machines aren't needed unless necessary
- Centralized coordinator has scalability bottleneck because need 1 round-trip for invocation and get
	- Solution is to shard by ownership. Caller of function is owner of returned future, but value is stored at worker than runs the task. Advantage - less communication needed
- Failures are handled by the owners. Tasks re-executed following lineage If there's dangling refs, the children will self-destruct, called fate sharing.

### Secure Untrusted Data Repository (SUNDR)
- Servers can be malicious
- Servers can return anything, but due to digital signing by each client with their private key, WLOG servers only return current or prior versions of the filesystem
- Fork consistency means if client A's updates hidden from B, then client A and B must be forked, updates concealed
- Global lock
	- Log approach - log reads and writes to the filesystem, hash the whole log. Inefficient as log grows. 
	- Tree approach - merkle tree for each user, with the version vectors of each user's merkle tree.

### Bitcoin
- a transaction can have multiple ins and outs
- bitcoin is technically not anonymous, mapping from your info to your address
- double spending is a problem and solved by making people consider the longest history as real

### Practical Byzantine Fault Tolerance
- there are 3 stages - preprepare, prepare, commit
- you need 3f+1 replicas if there's f faulty replicas
- a view-change is when the primary changes. primaries can be faulty.