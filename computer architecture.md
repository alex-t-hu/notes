>**cache line** - valid/dirty bit, tag currently stored, access info used by LRU/FIFO policies, coherence state (multicore). a memory address split into **tag (id for the block that's stored), index (which cache line the block is stored in), block offset**. the index indexes into the set; the set stores **ways** elements. 32/64/128 bytes per block.
>**__builtin_prefetch(addr)** pre-fetches cache line at addr. also the L1 or L2 prefetchers will perform prefetching automatically, and typically 10 L1 LFBs (can prefetch 10 cache lines at a time) and 24 L2 LFBs. Latency of L1, L2, L3, memory is 1.6ns (3 cycles), 5.6ns, 28ns, 120ns, and capacity of L1, L2, L3 cache is 32KB, 1MB, 32MB
>**funnel-sort** sorts $n^{1/3}$ groups of $n^{2/3}$ items and merges. Cache Complexity is $n/B\log_{M}n$ where $B$ is cache line size in elements and $M$ is cache size in elements, which is asymptotically optimal and improves upon naive mergesort by $\log M$ 
>**CAS(*result, old, new)*** compares old to \*result and sets \*result to new if equal
>- **lock-free** push and pop with CAS
>- optimize further by testing if \*result is old and then doing the CAS, and exponential backoff
>**MESI** protocol
>- **Modified** - cache line is only in current cache and dirty
>- **Exclusive** - cache line is only in current cache, but is clean
>- **Shared** - may be stored in other caches and is clean
>- **Invalid** 
>**memory ordering** in multithreaded programs is saying that loads and stores can happen out of order. solved by putting fences, and additionally may need to use **volatile** keyword to tell compiler that other threads are changing variabels
>**false sharing**
>**ABA** problem in multi-threaded computing
>- in a lock-free DS, when item is removed, deleted, and new item is added, it is common for new item to be at same location, causing problems