
#### Queries so far

SELECT
FROM
WHERE
GROUP BY
HAVING


#### Order


##### Syntax for Order BY
- ORDER By order_item
    - order_item: column direction
    - direction: asc or desc
    - ORDER BY COUNT(*) desc
- orders result rows by values in order items
- Prioritize order items that appear earlier in list
- Applied after grouping
    - items must have unique value per group


#### Limit

- LIMIT number: shows first number of results
- applied at the very end


#### Unknown Values

- unknown values are called NULL
- SQL uses Ternary (three valued logic)
    - outcome may be True, false or unknown
- Check for corresponding outcome
    - expression = true
    - expression = false
    - expression IS NULL (this is not the same as " = NULL")
- WHERE condition evaluates to NULL ( no result row)



create table vgsales ( gamerank int, name varchar(255), platform varchar(50), year int, genre varchar(50), publisher varchar(50), nasales decimal(10,4), eusales decimal(10,4), jpsales decimal(10,4), othersales decimal(10,4), globalsales decimal(10,4));


load data infile '/var/lib/mysql-files/vgsales.csv' 
into table vgsales 
fields terminated by ','  
enclosed by '"'  
lines terminated by '\n'  
(gamerank, name, platform,
@year, genre, publisher, nasales, eusales, jpsales, othersales, globalsales) 
set year = NULLIF(@year, 'N/A');


#### Joins with Unknowns
- Standard join keeps only the matching row pairs
- eliminates rows without matching rows in other table

- if we want to keep rows regardless
- we can use outer joins
- fields in missing rows are filled with null

##### Left Outer Join
- keep each row in left table ( plus standard join result)
    - table1 LEFT OUTER JOIN table2 ON....

##### Right Outer Join
- keep each row in right table (plus standard join result)
    - table1 RIGHT OUTER JOIN table2 ON ...

##### Full Outer join
- keep rows in both tables (plus standard join result)
    - table1 FULL OUTER JOIN table2 ON ...


SELECT sname, count(cid)
    FROM students LEFT OUTER JOIN enrollment
    ON (students.sid = enrollment.sid)


#### Set Operations

##### Union
- union result tuples from two queries
    - query1 UNION query2 
        eliminates duplicates
    
    - query1 UNION ALL query2
        keeps duplicates

##### Intersect
- results from two queries that intersect
    - query1 INTERSECT query2

##### Except
- set difference between queries
    query1 EXCEPT query2

- Results from query1 and query2 must be union compatible
    - same no of columns
    - same column types


#### Query Nesting

- use queries as part of another query
    - query instead of table in FROM clause
    - query instead of conjunct in WHERE clause

Query (containing query)
sub-query (contained query)

correlated vs uncorrelated sub-queries (query that can run independently)

correlated sub-queries reference containing query


SELECT sname FROM students
WHERE gpa >= (SELECT MAX(gpa) FROM students)


##### sub-queries in conditions

###### EXISTS
- check if sub-query result is empty
- EXISTS (sub-query) : True if not empty

###### IN
- check if sub-query result contains value
- value IN (sub-query) : True if contained

###### ALL and ANY
- check if condition holds for all/some of the sub-query
- value >= ALL (sub-query) : True if satisfied for all
- value >= ANY (sub-query) : True if satisfied for any

SELECT sname from students 
WHERE gpa >= ALL (SELECT gpa FROM students)



##### Corelated sub-query

SELECT s1.sname FROM students as s1
WHERE s1.gpa >= ALL (SELECT s2.gpa FROM students as s2 WHERE s1.sname = s2.sname) 

###### Evaluating corelated sub-queries
- iterate over rows from outer query
- evaluate sub-query for fixed row in outer query
- decide whether row belongs in result

SELECT s1.sname FROM students as s1 
WHERE EXISTS  (SELECT s2.gpa FROM students as s2 WHERE s1.gpa < s2.gpa)


##### Multiple Nesting Levels
SELECT c.cname FROM courses as c 
WHERE NOT EXISTS ( SELECT * FROM students as s 
                WHERE NOT EXISTS  ( SELECT * from enrollment as e 
                                            WHERE e.cid = c.cid AND E.sid = s.sid))
