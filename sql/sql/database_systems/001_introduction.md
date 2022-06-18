
#### SQL Command Types
- DDL: Data Definition Language
    - Define admissible database content (Schema)
- DML: Data Manipulation Language
    - Change and Retrieve Database content
- TCL: Transaction Control Language
    - Groups SQL commands (Transactions)
- DCL: Data Control Language
    - Assign Data access rights



#### Schema Definition
CREATE TABLE table_name (table_def)
table_def = comma separated column definitions
column definition is of type column_name column_type

#### Constraints
- constraints that limit admissible content of tables
- DBMS enforces integrity constraints

##### Ways to add constraints
- Can be added via ALTER TABLE command
- can be defined while creating the table

##### Types of Constraints
- Primary Key constraints
    - refers to a single table
    - defines a subset of columns as key columns
    - fixing values for key columns must identify row
    - No two rows have same values in key columns
    - ALTER TABLE table_name ADD CONSTRAINT Primary Key (key_columns)
        - key_columns is a comma separated list of column names
        - columns must exist
        - ALTER TABLE students ADD Primary Key (id)

- Foreign Key constraint
    - links two tables
    - identifies a set of foreign key columns in table 1
    - maps foreign key columns in table 1 to primary key columns in table 2
    - values in foreign key columns must appear as primary key 
    - maps each row in table 1 to a row from table 2
    - ALTER TABLE table_1 ADD CONSTRAINT Foreign Key (fkey_columns) REFERENCES table_2 (pkey_columns)
        - table_1: table with foreign key
        - table_2: table with primary key
        - fkey_columns: comma separated foreign key columns
        - pkey_columns: comma separated primary key columns
        - ALTER TABLE enrollment ADD Foreign Key (cid) REFERENCES courses (cid)


create table student (s_id int, s_name text);
create table course (c_id int, c_name text);
create table enrollment (s_id int, c_id int);
alter table student add primary key (s_id);
alter table course add primary key (c_id);
alter table enrollment add primary key (s_id, c_id);
alter table course add foreign key (s_id) references student (s_id);
alter table course add foreign key (c_id) references course (c_id);


#### Inserting Data

##### Fully specified row
- INSERT INTO table_name VALUES (values_list)
    - values_list: comma separated values

##### Partial columns
- INSERT INTO table_name (column_list) VALUES (values_list)
    - values_list: comma separated values
    - column_list: Comma separated column names

##### Inserting Data from File
- COPY table_name FROM path DELIMITER delimiter NULL null_string CSV

#### Delete Data

##### Satisfying a condition
- DELETE FROM table_name WHERE condition
    - condition specifies boolean predicate








