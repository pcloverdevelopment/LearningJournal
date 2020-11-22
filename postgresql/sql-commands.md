# SQL Commands

## Categories of SQL Commands

- Data Definition Language (DDL)
  - CREATE
  - ALTER
  - DROP
  - RENAME
  - TRUNCATE
  - COMMENT
- Data Querying Language (DQL)
  - SELECT
- Data Manipulation/Modification Language (DML)
  - INSERT
  - UPDATE
  - DELETE
  - MERGE
  - CALL
  - EXPLAIN PLAN
  - LOCK TABLE
- Data Control Language (DCL)
  - GRANT
  - REVOKE

### SQL Command Examples & Snippets
- Renaming columns on select can be done via: 
  - `SELECT column as "<new name>"`
  - Good for creating clearer columns names for reports.
- Column Concatenation can be done via:
  - `SELECT CONCAT(emp_no, ' is a ', title) AS 'Employee Title' FROM public.titles`
  - Good for combining normalized data into groupings that make sense.

### Filtering Data

#### Boolean Algebra Operators
> Use the `WHERE` clause to filter the data of a SQL query..

> In the `WHERE` clause we can use the `AND` along with the `OR` to build up a more complex clause. This is boolean algebra at work.

> Another boolean operator we can use is the `NOT` operator. 

#### Comparison Functions and Operators
> Some of the operators we have are **<, >, <=, >=, =, <>, !=** (last two are both not equal to)

#### Logical Operators
> Remember **AND, OR, & NOT** we need to pay attention to a few things when filtering:

- When SQL is evaluating our queries, the order it does so is:
  1) **FROM** what table?
  2) **WHERE** what is true?
  3) **SELECT** all data that meets the first two.

- We also have operator precedence to consider along with associativity. Review the documentation as needed. Just remember, operator precedence is evaluated first, then the associativity is evaluated so left to right, or right to left evaulation. Use the following quick guide if need be for Highest to Lowest precedence:
    1) Parenthesis
    2) Arithmetic Operators
    3) Concatenation Operators
    4) Comparison Conditions
    5) IS NULL, LIKE, NOT IN, etc.
    6) NOT
    7) AND
    8) OR

#### NULL values and checking for NULL
> **NULL** is the term used to represent a missing or empty value.

> A NULL value is different from a zero value or a field that contains spaces.

Always remember to be careful, mindful, and deliberate when we use null. Don't just make something null because you don't know what to do it. Make something null on purpose!

It's often a contentious issue, so people often disagree around the idea of using null in the database.

> No matter what we do with null, it will always be null (subtract, divide, equal, etc...) which can cause some quirky bugs. 





