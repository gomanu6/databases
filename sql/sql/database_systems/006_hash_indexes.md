

#### Hash vs tree Indexes
- Tree Index: traverse search tree to find interesting leafs
- Hash Index: evaluate hash functions to find buckets


#### Supported Predicates : Tree Index
- tree indexes are based on sort order of keys (comparison between keys)
- can handle equality and inequality conditions
    - consecutive keys stored together
- composite keys: useful for conditions on key prefix
    - keys with same prefix value stored close together


#### Supported Predicates : Hash Indexes

- Hash Indexes are based on key hash values
    - only useful for equality conditions
    - consecutive keys may be sorted far apart
    - similar hash value not equal similar key value
- condition must constrain all components
    - keys with same prefix may be stored far apart


#### Hash Index variants
- Static hashing
    - bad for dynamic data
- extendible hashing
    - expands with few high overhear ops
- linear hashing
    - expands more smoothly


##### Static Hashing
- hash bucket pages contain references to data
    - alternatively may contain data directly
- hash buckets are associated with hash value ranges
- can use hash index to find entries with key V
    - calculate hash value h for V as h(V)
    - lookup bucket page associated with h

###### Static hashing pro/cons
- can get data with one read
- may need multiple reads in case of overflow pages
- will waste space if too many deletions (empty pages)
- can use rehashing but creates significant overheads


##### Extendible hashing
- use directory to map hash buckets to pages
    - more flexible than using pageids directly
- redistribute overflowing buckets to multiple pages
    - more efficient than having to rehash all data
need to increase directory size if too many splits


###### Terminology

Global depth: how many hash bits directory considers
Local depth: How many hash bits for specific bucket

###### Extendible hashing pro/con
- avoids overflow pages
- no need for expensive rehashing
    - only rehash one bucket at a time
- need additional directory access
- need to double directory occasionally
    - this may take up some time

##### Linear Hashing
- avoid directory by fixing next bucket to split
- means we do not always split overflowing bucket
- we may have temporary overflow pages
- buckets to split are selected in round robin fashion
- means overflowing bucket will be split eventually

###### Insertion summary linear hashing
- calculate hash value for new entry to insert
- add entry in page, or if necessary, on overflow page
- split next bucket, if trigger condition is satisfied
    - may eliminate previously generated overflow pages
    - some flexibility on choice of trigger condition


###### Splitting summary
- splitting proceeds in rounds
    - all buckets present at round start split by round end
    - next split pointer is reset to first page at round end
- We always split the bucket pointed to by next split
    - add one new page, redistribute split bucket entries
    - consider one more bit when redistributing

###### Linear Hashing pro/con
- avoids a directory - no expensive directory doubling
- may temporarily admit overflow pages
- may split empty pages - inefficient space utilisation


##### Optimisations
- same optimisations as tree indexes
- have many entries for same search key value?
    - store key value followed by list of references
- want to get rid of one level of indirection?
    - can store data directly instead of references
        - leads to clustered index, only one per table
        - clustered index in general: data sorted by index key



#### Choose index type in postgres
- CREATE INDEX index_name ON table USING method (column_list)
    - method: btree or hash
    - other choices as well




    


