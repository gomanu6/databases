


RG sec 9
#### Indexes

- Index: auxiliary data structure for finding data faster
- can have multiple indexes 

- Index stores references to data records slotid and pageid
- index groups records by values in specific columns
    - these columns are called index search keys
    - index retrieves records for specific search key values


##### Tree Index


###### Where to use tree indexes

- queries with equality predicates
    - WHERE sname = 'Alan'

- queries with inequality predicates
    - WHERE gpa > 3

- both cases work if predicate references index key


#### Composite Keys
- Index Search key may consist of multiple columns
- must decide priority order between key columns
- key comparisons use the priority order
- can use index for inequalities on prefix of key columns


#### Indexes in Postgres

- CREATE INDEX index_name ON table (columns)
    - refer to index later via index_name
    - columns is comma separated columns list (key)

- DROP INDEX index_name
    - deletes index


\d tablename
\timing

#### Which Index to create
- depends on typical queries
- analyse prediactes of queries
- too many indexes are bad for performance
- tools
    - [Dexter :](https://ankane.org/introducing-dexter)



#### Optimisation for Indexes

##### Concise data entries
- Many references to same search key value?
    - Optimisation: Store search key value with reference list
    - Advantage: avoids storing key values redundantly
    - Disadvantages: creates variable length field (list)


##### Merging Index and Data
- Idea: Index stores data instead of references to data
    - this is called clustered index
- can have at most one clustered index per table
- more efficient as it saves chasing one reference
- collocates data with same key


#### Tree index variants
- B+ tree index, focus so far
- some tree indexes put references in inner nodes
- some omit links between leaf nodes
- ...
- differences in how updates are handled


#### Problems
- overflow pages reduce performance
- empty pages leads to space overhead

#### B+ Trees
- most popular index structure
- default in postgres
- balances tree after insert/delete operations
- keeps tree compact
    - each node, except root, is atleast half full
    - number entries between d and 2d (d is order)
    - order: minimum entries per node

##### B+ Trees are shallow
- typical order is 100, typical fill factor is 67%
- average fanout, no of child nodes is 133
- second level can have 133^2 = 17,689 nodes
- third level can have 133^3 = 2,352,637 nodes
- fourth level can have 133^4 = 312,900,721 nodes













