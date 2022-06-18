


#### SQL Queries
- An sql query describes a new relation to generate

##### Simple SQL queries have three parts
- SELECT: describes columns of relation to generate
- FROM: describes source relations and how to match
- WHERE: defines conditions result rows must satisfy


##### Simple Query format
- SELECT columns FROM table1 JOIN table2 ON  (join_cond) WHERE (where_cond);
    - columns: comma separated list of columns
    - table1 abd table2: database relations or tables
    - join_cond: condition defining matching tuples pairs
    - where_pred: additional conditions

- SELECT students.sid 
    FROM students 
        JOIN enrollment on (student.sid = enrollment.sid) 
        JOIN courses on (enrollment.cid = courses.cid) 
    WHERE courses.cname = 'CS4320';


SELECT courses.cname
    FROM courses
        JOIN enrollment on (courses.cid = enrollment.cid)
        JOIN students on (enrollment.sid = students.sid)
    WHERE students.sname = 'John Smith';

#### Simplification by Alias 
SELECT s.sid 
    FROM students as s
        JOIN enrollment as e on (s.sid = e.sid) 
        JOIN courses as c on (e.cid = c.cid) 
    WHERE c.cname = 'CS4320'; 

If column names are unique the table name need not be prefixed


#### Diverse predicates
- can use inequalities such as > and >=, <, <=
- not equal : <>
- check if value in list: cname IN ('CS4320', 'CS5320')
- Regex: cname LIKE 'CS_320%'
    - % zero or more arbitrary characters
    - _ one arbitrary character


#### Composite Predicates
- Logical conjuction: AND
- disjunction: OR
- Negation: NOT


#### Select clauses
- * : Select all columns
- table.* : Selects all columns form table
- arithmetic expression in select 
    - SELECT 3* (column_1 + column_2)
- assign new names for output columns
    - SELECT sname AS Student_Name


#### Join Syntax Alternatives

- Simply specify names of columns that appear in multiple tables
    - table1 JOIN table2 USING (column)

- Natural Joins
    - table1 NATURAL JOIN table2
    - introduces equality conditions between columns of same name

- No JOIN keyword
    - FROM table1, table2 WHERE join_condition

#### Select Distinct
- SELECT DISTINCT eliminates duplicates



#### Aggregation Queries

##### Aggregates
- SUM
- COUNT
- AVG
- MAX
- MIN
- GROUP BY

- SUM, AVG, MIN, MAX : Numerical expression parameter
- COUNT(*) : for counting rows in result
- COUNT(column) : counts rows with value in column
- COUNT DISTINCT(column) : counts number of distinct values


SELECT COUNT(*) 
    FROM students 
        JOIN enrollment on (student.sid = enrollment.sid) 
        JOIN courses on (enrollment.cid = courses.cid) 
    WHERE courses.cname = 'CS4320';

##### Aggregate and Grouping

- GROUP BY 
    - GROUP BY column
    - distinguish data subsets based on their values in specified column


SELECT COUNT(*), cname      // count rows in each group and report count with course name (unique per group)
    FROM students
        JOIN enrollment ON (students.sid = enrollment.sid)
        JOIN courses ON (enrollment.cid = courses.cid)
    WHERE cname IN ('CS4320', 'CS5320')
    GROUP BY cname

- Grouping is applied after pairing data sources (FROM) and filtering rows (WHERE)
- Result contains one row per group
    - implies restriction on SELECT clause
    - only expressions with unique value per group
    - This includes aggregates and grouping columns


##### Predicates on Groups

- HAVING
    - specifies conditions on groups ( evaluated after grouping )

SELECT COUNT(*), cname      // count rows in each group and report count with course name (unique per group)
    FROM students
        JOIN enrollment ON (students.sid = enrollment.sid)
        JOIN courses ON (enrollment.cid = courses.cid)
    WHERE cname IN ('CS4320', 'CS5320')
    GROUP BY cname
    HAVING COUNT(*) >= 100


