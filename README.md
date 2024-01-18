## Solution

## Chapter 3: Single Row Functions

### 3.3

a. For all employees, find the number of years employed. Print first names and number of years
employed for each employee.

```js
SELECT FIRST_NAME, MONTHS_BETWEEN((SYSDATE – HIRE_DATE)) / 12 “Years employed”
FROM employees;

```

b. Suppose you need to find the number of days each employee worked during the first month
of his joining. Write an SQL query to find this information for all employees
```js
SELECT FIRST_NAME, 30 - EXTRACT(DAY FROM HIRE_DATE) "Days Worked in First Month"
FROM employees;

```

### 3.4

a. Print the commission_pct values of all employees whose commission is at least 20%. Use NVL function.

```js
SELECT FIRST_NAME, COMISSION_PCT
FROM employees
WHERE NVL(COMISSION_PCT, 0) >= 0.2;

```

b. Print the total salary of an employee for 5 years and 6 months period. Print all employee last
names along with this salary information. Use NVL function assuming that salary may
contain NULL values

```js
SELECT FIRST_NAME, (SALARY * 66 + SALARY * 66 * NVL(COMISSION_PCT, 0)) "Salary"
FROM employees;
```

### Note: Converting date


TO_CHAR(HIRE_DATE, ‘DD/MM/YYYY’)    ->     ‘31/01/1995’;
TO_CHAR(HIRE_DATE, ‘MONTH DD, YYYY’)   ->  ‘JANUARY 31, 1995’;
TO_CHAR(HIRE_DATE, ‘MONTH DD, YEAR’)  ->   ‘JANUARY 31, NINETEEN HUNDRED AND NINETY FIVE';
TO_CHAR(HIRE_DATE, ‘Ddspth Month, YEAR’) -> ‘Thirty-First January, Nineteen hundred and ninety five’;
TO_CHAR(123456)            ->              ‘123456’;

### 3.5

Example 1. Finds all employee last names who was hired before 1st January 1997.

```js
SELECT LAST_NAME, TO_CHAR(HIRE_DATE, 'DD-MON-YYYY') "Hire Date"
FROM employees
WHERE HIRE_DATE < TO_DATE('01-JAN-1997', 'DD-MON-YYYY')
ORDER BY HIRE_DATE ASC;
```

#### Exercise

a. Print hire dates of all employees in the following formats:
(i) 13th February, 1998 (ii) 13 February, 1998.

i.
```js
SELECT LAST_NAME, TO_CHAR(HIRE_DATE,'ddth Month, YYYY') "Hire Date"
FROM employees
ORDER BY HIRE_DATE ASC;
```

ii.
```js
SELECT LAST_NAME, TO_CHAR(HIRE_DATE,'dd Month, YYYY') "Hire Date"
FROM employees
ORDER BY HIRE_DATE ASC;
```


## Chapter 4: Aggregate Functions

### 4.1

Example 1. Find the maximum salary, minimum salary and average
salary the company pays in different job types. 

```js
SELECT JOB_ID, MAX(SALARY), MIN(SALARY), AVG(SALARY)
FROM employees
GROUP BY JOB_ID;
```

Example 2. Find the number of employees working in each department.

```js
SELECT DEPARTMENT_ID, COUNT(*)
FROM employees
GROUP BY DEPARTMENT_ID;
```

Example 3. Find the maximum and minimum salary for each job types for only the
employees working in the department no. 80. Sort based on job type.

```js
SELECT JOB_ID, MAX(SALARY), MIN(SALARY)
FROM employees
WHERE DEPARTMENT_ID = 80
GROUP BY JOB_ID
ORDER BY JOB_ID ASC;
```

#### Exercise

a. For all managers, find the number of employees he/she manages. Print the MANAGER_ID and total number of such employees.

```js
SELECT MANAGER_ID, COUNT(*)
FROM employees
WHERE MANAGER ID IS NOT NULL
GROUP BY MANAGER_ID;
```

b. For all departments, find the number of employees who get more than 30k salary. Print the
DEPARTMENT_ID and total number of such employees

```js
SELECT DEPARTMENT_ID, COUNT(*)
FROM employees
WHERE SALARY > 30000
GROUP BY DEPARTMENT_ID;
```

c. Find the minimum, maximum, and average salary of all departments except
DEPARTMENT_ID 80. Print DEPARTMENT_ID, minimum, maximum, and average salary.
Sort the results in descending order of average salary first, then maximum salary, then
minimum salary. Use column alias to rename column names in output for better display.

```js
SELECT DEPARTMENT_ID, MAX(SALARY) max_salary, MIN(SALARY) min_salary, AVG(SALARY) avg_salary
FROM employees
WHERE DEPARTMENT_ID <> 80
GROUP BY DEPARTMENT_ID
ORDER BY avg_salary DESC, max_salary DESC, min_salary DESC;
```

### 4.2

#### Exercise

a. Find for each department, the average salary of the department. Print only those
DEPARTMENT_ID and average salary whose average salary is at most 50k.

```js
SELECT DEPARTMENT_ID, ROUND(AVG(SALARY),2) avg_salary
FROM employees
GROUP BY DEPARTMENT_ID
HAVING avg_salary <= 50000;
```

### 4.3

#### Examples

Example 1. Find the total employees hired in each year.

```js
SELECT TO_CHAR(HIRE_DATE, 'YYYY') YEAR, COUNT(*) "Total Hired Employees"
FROM employees
GROUP BY TO_CHAR(HIRE_DATE, 'YYYY')
ORDER BY YEAR ASC;
```

Example 2. Find the total number of employees whose name starts with the same character.

```js
SELECT SUBSTR(FIRST_NAME, 1, 1) FC, COUNT(*) "Total Employees"
FROM employees
GROUP BY SUBSTR(FIRST_NAME, 1, 1)
ORDER BY FC ASC;
```

#### Exercise

a. Find number of employees in each salary group. Salary groups are considered as follows.
Group 1: 0k to <5K, 5k to <10k, 10k to <15k, and so on.


```js
SELECT TRUNC(SALARY / 5000, 0) + 1 AS SALARY_GROUP, (TRUNC(SALARY / 5000, 0) * 5 || 'k - ' || (TRUNC(SALARY / 5000, 0) + 1) * 5 || 'k') AS RANGE, COUNT(*) AS "Number of Employees"
FROM employees
GROUP BY TRUNC(SALARY / 5000, 0)
ORDER BY SALARY_GROUP ASC;
```

b. Find the number of employees that were hired in each year in each job type. Print year, job id,
and total employees hired.

```js
SELECT TO_CHAR(HIRE_DATE, 'YYYY') YEAR, JOB_ID, COUNT(*) "Total Hired Employees"
FROM employees
GROUP BY TO_CHAR(HIRE_DATE, 'YYYY'), JOB_ID
ORDER BY YEAR ASC, JOB_ID ASC;
```
