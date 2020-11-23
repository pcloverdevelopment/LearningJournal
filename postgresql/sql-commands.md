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

#### NULL values & checking for NULL with IS keyword
>Basic example to check if something is(n't) null:

```sql
SELECT *
FROM <table>
WHERE <field> IS [NOT] NULL;
```
> What it does: *Checks if a column is or isn't null*.

> **NULL** is the term used to represent a missing or empty value, and it is different from a zero value or a field that contains spaces. The relational operators such as **=** and **!=** doesn't work with **NULL** so we have to use **IS [NOT]**
> 
#### NULL Coalescing
>Basic example:

```sql
SELECT coalesce(<column>, 'Empty') AS column_alias
FROM <table>;
```
> What it does: *Coalesce returns the first non-null value in a list*... so it returns the first non-null or replaces it with what we passed in as the second argument.

We can also use Combine Coalesce which allows us to use multiple columns as parameters and it goes in order through them..

#### BETWEEN ... AND
>Basic example:

```sql
SELECT <column>
FROM <table>
WHERE <column> BETWEEN X AND Y;
```
> What it does: *Evaluates to true for all rows where column is x or y or in the range between and x and y*

#### BETWEEN ... AND
>Basic example:

```sql
SELECT <column>
FROM <table>
WHERE <column> IN (<x_1>, ..., <x_n>);
```
> What it does: *Evaluates to true for all rows where column is one of the listed values*

The IN keyword can help us use a range of values in a search without a bunch of repetitive OR operator chaining

#### LIKE keyword ... & ILIKE keyword!
>Basic example:

```sql
SELECT <column>
FROM <table>
WHERE <column> LIKE <Some_string_with_pattern_matching>;
```
> What it does: *Helps us do a partial search.*
> **We can replace the LIKE above with ILIKE to do a case insensitive matching**

- Pattern matching wildcards:
  - *%* can be used to match 0 or more characters
  - *_* can be used to match any 1 character

POSTGRES only does text comparison with the LIKE operator, so if we want to use it the attribute being used in the search must be cast to text.

##### Casting something to Text
Two ways:

```sql
CAST(salary AS text);
```
**OR**
```sql
salary::text -- a bit less explicit
```

#### Dates & Time zones
```sql
SET TIME ZONE 'UTC'; -- how to configure PSQL to use UTC
```
```sql
SHOW TIMEZONE; -- ask PSQL for it's timezone
```
> Greenwich Mean Time (GMT) is a time zone

> Universal Time Coordinate (UTC) is a standard to represent time

They may both share the same current time, but they are conceptually different. One is a time zone so it belongs to a Territory but UTC is a standard for time that no territory uses.

Representing dates as a standard such as UTC allows us to convert it to whatever time zone we need be because we are using a normalized/standard approach.

>**POSTGRES is a database that stores everything as UTC by default, but will often show you your local time zone on selects on that data, so it's doing the conversion from UTC to your timezone**

```sql
-- change a user's settings permanently so all
-- future sessions use this timezone.
ALTER USER <user_name> SET timezone='UTC'
```

##### Formatting Date and Time
> POSTGRESQL USES ISO-8601 to describe formatting dates and times.. notice another standard to just follow.

```
/* ISO-8601 format*/
YYYY-MM-DDTHH:MM:SS
Example: 2017-08-17-T12:47:16+02:00
```
The above example has an offset (*+02:00*) that says this time is +2 hours from the UTC standard of time, but if the offset wasn't there then POSTGRES would assume it was already in UTC time.

##### Timestamps
> A timestamp is a date with time and possibly time zone info.

```sql
SELECT now() -- get the current time in the db system
```

Postgres has two ways of storing timestamps:
```sql
-- example of how we create timezones with SQL
CREATE TABLE timezones (
  ts TIMESTAMP WITHOUT TIME ZONE,
  tz TIMESTAMP WITH TIME ZONE
);
```
*Pay close attention to how we have defined our timestamp columns along with the representation of time we are storing in the DB*

#### Postgres operators for working with dates

Few snippets:
```sql
-- both get the current date in an ISO-8601 format
SELECT NOW()::date;
SELECT CURRENT_DATE:
```
```sql
-- convert the ISO-8601 format to something else.
-- postgres docs have a bunch of documentation
-- for formatting dates
SELECT TO_CHAR(CURRENT_DATE, 'dd/mm/yyyy');
```
```sql
-- gets the difference between dates
-- returns format of: <no> days HH:MI:SS.MS
SELECT NOW() - '1800/01/01';
```
```sql
-- casts a string to a date to ISO-8601
SELECT date '1800/01/01';
```
```sql
-- get the years months and days since a specified time
-- returns format of: <no> years <no> mons <no> days
SELECT AGE(date '1800/01/01');
```
```sql
-- get the age between two dates in ISO-8601
-- returns format of: <no> years <no> mons <no> days
SELECT AGE(date'1992/11/13', date '1800/01/01');
```
```sql
-- extracting information
-- can replace YEAR with DAY or MONTH
SELECT EXTRACT (YEAR FROM date '1992/11/13') AS col_alius;

-- truncating information
-- can replace 'year' with 'day' or 'month'
SELECT DATE_TRUNC('year', date '1992/11/13') AS col_alius
```
```sql
-- use an interval when filtering
SELECT *
FROM order
WHERE purchaseDate <= now() - interval '30 days';
```

















