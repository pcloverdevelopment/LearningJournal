# SQL Functions

## What is a function when it comes to SQL?
> A function is a set of steps that creates a single value.

## Types of functions in SQL
- **Aggregate** functions take all of the rows in a given select query and combine them into one single output... for example, the `COUNT()` or `SUM()` functions are good examples of aggregate functions.
  - simply put, aggregate functions run against all the data as a whole for their input and output a single value.
  - Aggregate functions operate on many records to produce 1 value, or a summary of the data.
- **Scalar** functions are for outputting information on every single row of a select query... for example, the `CONCAT()` functions is a good example of a scalar function.
  - simply put, scalar functions run against each row of data as their input and output a value for each row of input.
  - Scalar functions operate on each record individually.

### Some aggregate functions we can use
- AVG()
- COUNT()
- MIN()
- MAX()
- SUM()

