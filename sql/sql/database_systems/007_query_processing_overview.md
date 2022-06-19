
RG section 7.4

#### Buffer Manager
- decides when to move data between disk and ram
- tries to reduce data movement by using heuristics
- manager buffer pool
    - buffer pool: main memory reserved for dbms
    - divided into page sized slots called frames
    - stores meta data about each slot


##### Frame Properties
- Page ID: which page is currently stored in frame
    - allows matching page request to frames
- Pin count: how many processes are using a page
    - can only evict page if pin count reaches zero
- Dirty bit: in-memory page deviates from disk version (any changes made to the page which would be lost in case of power failure)
    - must write page to  disk before evicting it


##### Processing Page Requests
- Case 1: Cache Hit
    - increase pin count and return page address
Case 2: Cache Miss
    - choose frame for repalcement (replacement policy)
    - If frame contains dirty page then write to disk
    - read requested page from diska nd store in frame
    - increase pin count and return page address


##### Why not rely on the OS

- dbms knows its access patterns ahead of time
- can exploit smarter replacements
- dbms must control page writes for safety gurantees


##### LRU Replacement Policy
LRU: Least recently used
- want to replace page required farthest in the future
- doing so reduces expensive cache miss
- difficult to predict that in general
- heuristic: remove lru
    - did not use page froa long time = unlikely to do soon


##### Sequential flooding
- dbms often has particular access patterns
- for instance: keep scanning pages in round robin mode
- lru page is used again soonest
- makes lru policy sub-optimal







