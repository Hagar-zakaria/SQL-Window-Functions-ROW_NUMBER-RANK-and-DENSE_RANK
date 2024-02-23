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
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/06ddcbd9-469a-4040-86f9-50f0d52eba35)

Let’s rank employees based on their salaries in descending order, and divide the result set into partitions based on the ‘position’ column.

```sql
SELECT *, ROW_NUMBER() OVER(ORDER BY salary DESC) AS new_employee_id
FROM employees;
```
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/e5e46b97-af80-4d96-91df-430c56dd1ba1)

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
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/11981535-a3e9-4fc0-890f-c17423692da2)

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
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/14ac7d31-a722-4dbb-a658-5eb1497eec1c)

Let’s divide the result set into partitions based on the ‘type’ column.

```sql
SELECT title, price, type, DENSE_RANK() OVER(PARTITION BY type ORDER BY price DESC) as 'rank'
FROM titles;
```
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/687de12b-9849-456f-bd94-5347cc8b79da)

Suppose we have a table called “titles” with columns “title” and “ytd_sales”. To identify the top-performing books, we can use the following query:

```sql
SELECT title, ytd_sales, DENSE_RANK() OVER(ORDER BY ytd_sales DESC) as 'rank'
FROM titles;
```
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/9fd3305e-eee7-497a-8d1b-5b629c541610)

Let’s divide the result set into partitions based on the ‘type’ column.

```sql
SELECT title, ytd_sales, type, DENSE_RANK() OVER(PARTITION BY type ORDER BY ytd_sales DESC) as 'rank'
FROM titles;
```
![image](https://github.com/Hagar-zakaria/SQL-Window-Functions-ROW_NUMBER-RANK-and-DENSE_RANK/assets/93611934/9e8b951f-3746-43d0-993a-5641bf8c243d)

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
