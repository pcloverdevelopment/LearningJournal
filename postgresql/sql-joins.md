# SQL JOINS

## Joins: Big Picture
> Think of joins as using the relationship between
> tables to extract and query for data that is from multiple tables. This gives us a great way to gain insight into data.

A join is just a combination of columns from one table with those of another, often through a relation of a primary key and foreign key. 

Postgre SQL JOINS:
- INNER (default)
- LEFT [OUTER]
- RIGHT [OUTER]
- FULL [OUTER]

#### Multi-table SELECT
>Basic example:

```sql
SELECT a.emp_no, 
        CONCAT(a.first_name, a.last_name) as "name", 
        b.salary
FROM employees as a, salaries as b
```
What it does: *Performs a cartesian product between both tables and runs the select.* **This is not imposing an constraint on something like a foreign key, so this query isn't very useful**

>A better example where we are filtering the data based on the relationship between the tables:

```sql
SELECT a.emp_no, 
        CONCAT(a.first_name, a.last_name) as "name", 
        b.salary
FROM employees as a, salaries as b
WHERE a.emp_no = b.emp_no;
```
What it does: *Performs a cartesian product between both tables, then it filters using the where, and runs the select.* **This is better but we have another syntax for performing this join, see inner join below.**

##### Inner join
>Basic example:

```sql
SELECT a.emp_no, 
        CONCAT(a.first_name, a.last_name) as "name", 
        b.salary
FROM employees as a
  INNER JOIN salaries as b ON a.emp_no = b.emp_no
```
What it does: *If you have a column(s) that links the columns from table A to table B you can use an inner join. This is the classic scenario where table A has a foreign key entry that maps to the primary key of table B.* 

> We are essentially saying select from employees as the main table, but also join salaries based on certain criteria... important to remember when visualizing queries that perform joins.

_NOTE:_ Even though we can perform the same join using the WHERE clause with comma on the FROM, the inner join syntax is generally considered a best practice because it's more readable than using the where syntax, and it shows you what's being joined.

```sql
-- an example of a 3 way join
SELECT a.emp_no,
       CONCAT(a.first_name, a.last_name) as "name",
       b.salary,
       c.title,
       c.from_date AS "promoted on"
FROM employees AS a
INNER JOIN salaries AS b ON a.emp_no = b.emp_no
INNER JOIN titles AS c ON c.emp_no = a.emp_no
AND c.from_date = (b.from_date + interval '2 days')
ORDER BY a.emp_no;
```

##### Self join
>Basic example:

```sql
-- self join
SELECT a.id, a.name as "employee", b.name as "supervisor name"
FROM employee as a, employee as b
WHERE a.supervisorId = b.id;
-- same query with inner join
SELECT a.id, a.name as "employee", b.name as "supervisor name"
FROM employee as a
INNER JOIN employee as b ON a.supervisorId = b.id;
```
What it does: *It is joining a table to itself, and it can usually be done when a table has a foreign key referencing its primary key. This seems quirky, but can be the case when you're working with a database that has designed it's schema that way.* 

It's really just another variant or subset of inner joins.

##### Outer join
>Basic example:

```sql
SELECT *
FROM <table A> AS a
LEFT [OUTER] JOIN <table B> AS b
ON a.id = b.id;
```
What it does: *Adds the data for rows that don't match on the specified side, left or right join, in the result set of the SELECT*

This join is for when we need the rows that don't match! Any values that don't match will be made NULL in the result set.

> Example question with query:

```sql
-- How many employees aren't managers?
SELECT COUNT(emp.emp_no)
FROM employees AS emp
LEFT JOIN dept_manager AS dep 
ON emp.emp_no = dep.emp_no
WHERE dep.emp_no IS NULL;
```

##### Less common joins
>CROSS JOIN basic example:

```sql
SELECT * 
FROM <table_a>
CROSS JOIN <table_b>
```
What it does: *Creates a cartesian product between the two tables.* 

>FULL OUTER JOIN basic example:

```sql
SELECT * 
FROM <table_a> AS a
FULL JOIN <table_b> AS b ON a.id = b.id;
```
What it does: *Returns results from both wether they match or not.* 

##### USING keyword
>Basic example:

```sql
-- USING syntax
SELECT e.emp_no, e.first_name, de.dept_no
FROM employees AS e
INNER JOIN dept_emp AS de USING(emp_no);
-- What the above syntax simplifies
SELECT e.emp_no, e.first_name, de.dept_no
FROM employees AS e
INNER JOIN dept_emp AS de ON e.emp_no = de.emp_no;
```
What it does: *Simplifies the join syntax*.

Basically just syntactic sugar around primary key & foreign key relationship mapping on our JOINS. I'd argue that ON is more readable and it's just more commonly used.