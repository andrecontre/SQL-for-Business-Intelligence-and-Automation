## 📑 Table of Contents
* [1. Basic Commands](#1-basic-commands)
* [2. Key Concepts Explained](#2-key-concepts-explained)
   * [The Rules of Aliases](#the-rules-of-aliases)
   * [Logic with CASE](#logic-with-case)
   * [Joins](#joins)
   * [Group by](#group-by)
* [3. Functions & Operators](#3-functions--operators)
* [4. Searching Text](#4-searching-text)
* [3. Example](#5-example)

# 1. Basic Commands
The fundamental building blocks of every query.

* `SELECT`: Specifies which columns to retrieve.
* `CASE -> END`: Handles "If-Then" logic within a query.
* `FROM`: Specifies the table to pull data from.
* `JOIN`: Used to combine rows from two or more tables based on a related column.
* `WHERE`: Filters rows based on a specific condition.
* `GROUP BY`: Collapses rows into summary categories (used with math functions).
* `ORDER BY`: Sorts the results 
  * `ASC`: in ascending order (by default).
  * `DESC`: in descending order.

## 2. Key Concepts Explained
### The Rules of Aliases
Aliases are "nicknames" for columns or tables.

| Type | Do you need `AS`? | Example |
| :--- | :--- | :--- |
| **Table Alias** | **No** | `FROM` customers c` |
| **Column Alias** | **Recommended** | `SELECT` name `AS` "Customer" (Makes headers readable) |
| **Calculations** | **Yes** | `SELECT` price * tax `AS` "Total"` |

> While `AS` is technically optional in many SQL versions for columns too, using it for **Column Aliases** is best practice because it makes it clear that you are renaming something. For **Table Aliases**, most developers just write **people p** because it's faster.
---
### Logic with `CASE`
The `CASE` statement is a "sandwich." You must start with `CASE` and end with `END`.

* **`WHEN`**: The condition you are testing.
* **`THEN`**: The result if the condition is true.
* **`ELSE`**: The default result if nothing matches.

---

### Joins
Joins link tables using a shared **"Key"** .

* **`INNER JOIN`**: Returns only the rows that have matching values in **both** tables.
* **`LEFT JOIN`**: Returns **everything** from the Left table, plus any matches found in the Right table.
* **`RIGHT JOIN`**: Returns **everything** from the Right table, plus any matches found in the Left table.

---

### Group By
`GROUP BY` is essential for "Math" in SQL. If you use aggregate functions like `COUNT`, `SUM`, or `AVG`, you must tell SQL which columns to "group" those totals into.

> If a column appears in your `SELECT` but is **NOT** being used in a math function (like `SUM`), it **must** be listed in your `GROUP BY` clause. If you leave one out, the query will fail.

## 3. Functions & Operators

### Aggregate Functions (The "Math" Helpers)
These functions perform a calculation on a set of values and return a single value. **Remember:** These require a `GROUP BY` clause for any non-aggregated columns.

| Function | Description |
| :--- | :--- |
| `COUNT()` | Returns the number of rows. |
| `SUM()` | Returns the total sum of a numeric column. |
| `AVG()` | Returns the average value. |
| `MIN()` / `MAX()` | Returns the smallest / largest value. |


### String Functions (Text Manipulation)
Used to clean or format text data.

* `UPPER(text)` / `LOWER(text)`: Converts text to all caps or all lowercase.
* `CONCAT(a, b)`: Joins two or more strings together (e.g., First Name + Last Name).
* `TRIM(text)`: Removes extra spaces from the start and end.
* `LENGTH(text)`: Returns the number of characters in a string.

### Date Functions
Working with time can be tricky; these functions help extract specific parts of a date.

* `CURRENT_DATE`: Returns today's date.
* `EXTRACT(YEAR FROM date_column)`: Pulls just the year (or month/day).
* `DATEDIFF(end, start)`: Calculates the number of days between two dates.

---

### Comparison & Logical Operators
Used in the `WHERE` clause to filter data.

| Operator | Meaning |
| :--- | :--- |
| `=` , `!=` | Equal to , Not equal to |
| `>`, `<` | Greater than, Less than |
| `AND` | Both conditions must be true. |
| `OR` | Either condition can be true. |
| `IN (v1, v2)` | Matches any value in a list (e.g., `country IN ('Mexico', 'Canada')`). |
| `BETWEEN x AND y` | Matches values within a range (inclusive). |

---
## 4. Searching Text
When filtering text in your `WHERE` clause, you have two main options:

| Operator | Match |
| :--- | :--- |
| `=`| Exact Match (includes capitalization) |
| `LIKE` | Fuzzy Match (us the % wildcard as placeholder for anything) |

```
Exact Match: 
WHERE country = 'France'; -- Will NOT find 'FRANCE' or 'france'

Fuzzy Match:
WHERE name LIKE :
Starts with: 'A%' (Finds 'Apple', 'Alphabet')
Ends with: '%ing' (Finds 'Running', 'Singing')
Contains:'%data%' (Finds 'database', 'metadata', 'dataset')

Example: Finding all emails that end in @gmail.com
SELECT * FROM users
WHERE email LIKE '%@gmail.com';

```

## 5. Example
This single block shows the correct syntax order and combines Aliases, Joins, Case logic, and Grouping.

```
SELECT 
    p.name AS "Employee",           
    d.dept_name AS "Department",
    
CASE 
    WHEN p.salary > 70000 THEN 'Senior'
    WHEN p.salary > 50000 THEN 'Mid-Level'
    ELSE 'Entry-Level'
END AS "Role_Level",

    COUNT(p.id) AS "Total_Staff"      

FROM people p                         
INNER JOIN departments d               
    ON p.dept_id = d.id                

WHERE p.status = 'Active'             

GROUP BY                               
    p.name, 
    d.dept_name, 
    p.salary                         

ORDER BY "Total_Staff" DESC; 
```

