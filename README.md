# SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK
This article explains SQL window functions ROW_NUMBER, RANK, and DENSE_RANK. It provides examples and syntax for each function and explains how to handle ties and gaps in ranking. The article also offers best practices for using window functions effectively. A must-read for SQL developers.

## 1. ROW_NUMBER Function : Creating Rankings
The ROW_NUMBER() function assigns a unique value to each row within a result set. This function is handy when you need to distinguish between rows but don’t require distinct ranking. It can be beneficial for pagination, data deduplication, or creating unique identifiers.

### General Syntax:

```sql
ROW_NUMBER() OVER (ORDER BY column)
```
```sql
`ORDER BY` specifies the column or expression used for ordering the result set.
```
```sql
ROW_NUMBER() OVER (PARTITION BY column ORDER BY column)
```
PARTITION BY is an optional clause that divides the result set into partitions based on the specified column. The ranking is applied separately within each partition.

Let’s assign employees with new employee IDs in descending order according to their salaries:


```sql
SELECT *, ROW_NUMBER() OVER(ORDER BY salary DESC) AS new_employee_id
FROM employees;
```

Let’s rank employees based on their salaries in descending order, and divide the result set into partitions based on the ‘position’ column.

```sql
SELECT *, ROW_NUMBER() OVER(ORDER BY salary DESC) AS new_employee_id
FROM employees;
```

## 2. RANK Function
The RANK() function assigns a unique rank to each row based on the values in one or more columns. Rows with the same values receive the same rank, and the next rank is skipped. It’s useful when you want to create a ranking with gaps.

### General Syntax:

```sql
RANK() OVER (ORDER BY column)
```

```sql
RANK() OVER (PARTITION BY column ORDER BY column)
```
Let’s rank employees in descending order according to their salaries:

```sql
SELECT * , RANK() OVER(ORDER BY salary DESC) AS salary_rank
FROM employees;
```

## 3. DENSE_RANK Function : Grouping Items with the Same Rank
DENSE_RANK() is useful when you want to group items with the same rank together. Rows with the same values receive the same rank, and the next rank is not skipped. This function is helpful when you want to create a ranking without gaps.

### General Syntax:

```sql
DENSE_RANK() OVER (ORDER BY column)
```
```sql
DENSE_RANK() OVER (PARTITION BY column ORDER BY column)
```
Suppose we have a table called “titles” with columns “title” and “price”.
Let’s say we want to rank book titles by their prices and group titles with the same sales prices:

```sql
SELECT title, price, DENSE_RANK() OVER(ORDER BY price DESC) as 'rank'
FROM titles;
```

Let’s divide the result set into partitions based on the ‘type’ column.

```sql
SELECT title, price, type, DENSE_RANK() OVER(PARTITION BY type ORDER BY price DESC) as 'rank'
FROM titles;
```

Suppose we have a table called “titles” with columns “title” and “ytd_sales”. To identify the top-performing books, we can use the following query:

```sql
SELECT title, ytd_sales, DENSE_RANK() OVER(ORDER BY ytd_sales DESC) as 'rank'
FROM titles;
```

Let’s divide the result set into partitions based on the ‘type’ column.

```sql
SELECT title, ytd_sales, type, DENSE_RANK() OVER(PARTITION BY type ORDER BY ytd_sales DESC) as 'rank'
FROM titles;
```

Combining All Three Functions
Consider a scenario where you have a list of employees with their salaries, and you want to assign a unique employee ID, rank them by salary, and assign a dense rank.
```sql
SELECT 
    first_name, last_name, position, salary,
    ROW_NUMBER() OVER (ORDER BY salary) AS employee_id,
    RANK() OVER (ORDER BY salary) AS salary_rank,
    DENSE_RANK() OVER (ORDER BY salary) AS dense_salary_rank
FROM employees;
```
In this example, we use all three window functions in a single query:

```sql
ROW_NUMBER() assigns a unique employee_id to each employee.
RANK() ranks employees by their salary, with gaps for identical salaries.
DENSE_RANK() ranks employees by their salary without gaps for identical salaries.
```
This query provides a comprehensive view of employee data, including unique identifiers and two types of salary rankings.

Conclusion
SQL window functions like ROW_NUMBER, RANK, and DENSE_RANK are indispensable tools for data analysis and reporting. They allow you to assign unique numbers or ranks to rows within a result set, making it easier to derive insights from your data.
