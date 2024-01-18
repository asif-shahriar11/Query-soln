## Solution

## Chapter 3

### 3.3

a. For all employees, find the number of years employed. Print first names and number of years
employed for each employee.

```
SELECT FIRST_NAME, MONTHS_BETWEEN((SYSDATE – HIRE_DATE)) / 12 “Years employed”
FROM employees;

```
