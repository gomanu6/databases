


#### Tables as files

- table schema information is stored in database catalog
- table content is stored in collection of pages (file)
- each page typically stores few Kb of data
- enough to store multiple rows but not entire table

##### Files to Pages

Possibility 1
- store pages as doubly linked lists
- each page contains pointers to next/previous page
- can use separate lists for full/partially empty pages
- reference to header page stored in db catalog

Possibility 2
- directory with pointer to pages
- directory pages reference data pages with meta pages


##### Pages to Slots
- Pages are divided into slots
- each slot stores one record (row)
- can refer to records via pageid and slotid
- multiple ways to divide pages into slots
- fixed length vs variable length records

###### Fixed Length records
- number of bytes per record is determined a priori
- need to keep track of which slots are used
- packed representation uses only consecutive slots
    - only keep track of number of slots used
- unpacked representation allows unused slots in between
    - need bitmap to keep track of used slots



###### Variable length records
- records with variable length text fields
- number of bytes per slot is not fixed a priori
- each page maintains directory about used slots
    - store first byte and length of slot
- flexibility to move around records on page
    - can use for regular compaction


##### Slots to fields
- must divide each slot into fields
- fixed length vs variable length fields
- Fixed Length: store field sizes in db catalog
- variable length: store field sizes on page
    - option1: use special delimiter symbol between fields
    - option2: store field directory at begining of record


tables -> pages -> slots -> fields

#### Row stores vs column stores
- data for same row is stored close together
- done by traditional dbms like postgres
- can also store data column wise
- data for same column is stored together
- can help if queries access only few columns


