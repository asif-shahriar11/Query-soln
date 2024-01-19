## Introduction

'A brief Introduction to Oracle-SQL-PL SQL', written by Prof. Sukarna Barua, is a SQL practice book, used in DB Sessional Lab in CSE, BUET.



## Chapter 3: Single Row Functions

### 3.3

a. For all employees, find the number of years employed. Print first names and number of years
employed for each employee.

```js
SELECT FIRST_NAME, MONTHS_BETWEEN((SYSDATE – HIRE_DATE)) / 12 “Years employed”
FROM employees;

```

b. Suppose you need to find the number of days each employee worked during the first month
of his JOINing. Write an SQL query to find this information for all employees
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



## Chapter 5: Join

### 5.1

Example 1. 

```js
SELECT E.LAST_NAME, E.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM employees E JOIN departments D
ON (E.DEPARTMENT_ID = D.DEPARTMENT_ID);
```

Example 2. Find the last name of manager for each employee.

```js
SELECT E1.LAST_NAME EMPLOYEE, E2.LAST_NAME MANAGER
FROM employees E1 JOIN employees E2
ON (E1.MANAGER_ID = E2.EMPLOYEE_ID);
```

Example 3. Create a report showing last name of an
employee and a number which is the number of employees getting higher salary than the employee. 


```js
SELECT E1.LAST_NAME NAME, COUNT(*) "Employees with Higher Salary"
FROM employees E1 JOIN employees E2
ON (E1.SALARY < E2.SALARY)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME
ORDER BY NAME;
```

Example 4. Joining more than two tables.

```js
SELECT E.LAST_NAME, D.DEPARTMENT_NAME, L.CITY
FROM employees E JOIN departments D
On (E.DEPARTMENT_ID = D.DEPARTMENT_ID)
JOIN locations L
ON (D.LOCATION_ID = L.LOCATION_ID);
```

#### Exercise

a. For each employee print last name, salary, and job title.

```js
SELECT E.LAST_NAME NAME, E.SALARY, J.JOB_TITLE
FROM employees E JOIN jobs J
ON (E.JOB_ID = J.JOB_ID);
```

b. For each department, print department name and country name it is situated in.

```js
SELECT D.DEPARTMENT_NAME, C.COUNTRY_NAME
FROM departments D JOIN locations L 
ON (D.LOCATION_ID = L.LOCATION_ID)
JOIN countries C
ON (L.COUNTRY_ID = C.COUNTRY_ID);
```

c. For each country, finds total number of departments situated in the country

```js
SELECT C.COUNTRY_NAME , COUNT(*) "Number of departments"
FROM countries C JOIN locations L
ON(C.COUNTRY_ID = L.COUNTRY_ID)
JOIN departments D
ON(L.LOCATION_ID = D.LOCATION_ID)
GROUP BY C.COUNTRY_NAME;
```

d. For each employee, finds the number of job switches of the employee.

```js
SELECT E.LAST_NAME, COUNT(J.JOB_ID) ""Job switches"
FROM employees E LEFT OUTER JOIN job_history J 
ON (E.EMPLOYEE_ID = J.EMPLOYEE_ID)
GROUP BY E.EMPLOYEE_ID, E.LAST_NAME;
```

e. For each department and job types, find the total number of employees working. Print 
department names, job titles, and total employees working.

```js
SELECT D.DEPARTMENT_NAME, J.JOB_TITLE, COUNT(*) "Num of employees"
FROM employees E JOIN jobs J
ON(E.JOB_ID = J.JOB_ID)
JOIN departments D
ON(E.DEPARTMENT_ID = D.DEPARTMENT_ID)
GROUP BY D.DEPARTMENT_NAME,J.JOB_TITLE
ORDER BY DEPARTMENT_NAME;
```

f. For each employee, find the total number of employees those were hired before him/her. Print employee last name and total employees.

```js
SELECT E1.LAST_NAME, COUNT(DISTINCT E2.EMPLOYEE_ID) "Employees hired before"
FROM employees E1 LEFT OUTER JOIN employees E2 
ON (E2.HIRE_DATE < E1.HIRE_DATE)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME;
```

g. For each employee, find the total number of employees those were hired before him/her and those were hired after him/her. 
Print employee last name, total employees hired before him, and total employees hired after him.

```js
SELECT E1.LAST_NAME, COUNT(DISTINCT E2.EMPLOYEE_ID) "Employees hired before", COUNT(DISTINCT E3.EMPLOYEE_ID) "Employees hired after"
FROM employees E1 LEFT OUTER JOIN employees E2 
ON (E2.HIRE_DATE < E1.HIRE_DATE)
LEFT OUTER JOIN employees E3
ON (E3.HIRE_DATE > E1.HIRE_DATE)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME;
```

h. Find the employees having salaries greater than at least three other employees.

```js
SELECT E1.LAST_NAME, COUNT(DISTINCT E2.EMPLOYEE_ID) "Salary greater than"
FROM employees E1 JOIN employees E2 
ON (E1.SALARY > E2.SALARY)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME
HAVING COUNT(DISTINCT E2.EMPLOYEE_ID) >= 3;
```

i. For each employee, find his rank, i.e., position with respect to salary. The highest salaried employee should get rank 1 
and lowest salaried employee should get the last rank. Employees with same salary should get same rank value. 
Print employee last names and his/he rank.

```js
SELECT E1.LAST_NAME, COUNT(DISTINCT E2.EMPLOYEE_ID) + 1 AS RANK
FROM employees E1 JOIN employees E2 
ON (E1.SALARY < E2.SALARY)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME
ORDER BY RANK ASC;
```

j. Find the names of employees and their salaries for the top three highest salaried employees.
The number of employees in your output should be more than three if there are employees with
same salary.


```js
SELECT E1.LAST_NAME, E1.SALARY, COUNT(DISTINCT E2.EMPLOYEE_ID) + 1 AS RANK
FROM employees E1 JOIN employees E2 
ON (E1.SALARY < E2.SALARY)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME, E1.SALARY
HAVING COUNT(DISTINCT E2.EMPLOYEE_ID) <=2
ORDER BY RANK ASC;
```

## Chapter 6: Query Multiple Tables – Sub-query

### 6.1: Sub-query

Example 1.  Find information of those employees whose JOB_ID is same as the employee numbered 141 and whose
SALARY is greater than Abel’s SALARY.

```js
SELECT EMPLOYEE_ID, LAST_NAME, SALARY
FROM employees
WHERE JOB_ID = (
    SELECT JOB_ID
    FROM employees
    WHERE EMPLOYEE_ID = 141
)
AND SALARY > (
    SELECT SALARY
    FROM employees
    WHERE FIRST_NAME = 'Abel'
); 
```

Example 2. Find the employee who gets highest salary among all employees.

```js
SELECT EMPLOYEE_ID, LAST_NAME, SALARY
FROM employees
WHERE SALARY = (
	SELECT MAX(SALARY)
	FROM employees
); 
```

Example 3. Find those employee records (working in other than ‘IT_PROG’ department) 
whose SALARY is less than at least one employee of ‘IT_PROG’.


```js
SELECT EMPLOYEE_ID, LAST_NAME, SALARY
FROM employees
WHERE JOB_ID <> 'IT_PROG'
AND SALARY < ANY (
	SELECT SALARY
	FROM employees
	WHERE JOB_ID = 'IT_PROG'
); 
```

#### Exercise

a. Find the last names of all employees that work in the SALES department.

```js
SELECT EMPLOYEE_ID, LAST_NAME
FROM employees
WHERE DEPARTMENT_ID = (
	SELECT DEPARTMENT_ID
	FROM departments
	WHERE DEPARTMENT_NAME = 'Sales'
); 
```

b. Find the last names and salaries of those employees who get higher salary than at least one
employee of SALES department

```js
SELECT LAST_NAME, SALARY
FROM employees
WHERE SALARY > ANY (
	SELECT SALARY
	FROM employees
	WHERE DEPARTMENT_ID = (
		SELECT DEPARTMENT_ID
		FROM departments
		WHERE DEPARTMENT_NAME = 'Sales'
	)
);
```

c. Find the last names and salaries of those employees whose salary is higher than all employees
of SALES department.

```js
SELECT LAST_NAME, SALARY
FROM employees
WHERE SALARY > ALL (
	SELECT SALARY
	FROM employees
	WHERE DEPARTMENT_ID = (
		SELECT DEPARTMENT_ID
		FROM departments
		WHERE DEPARTMENT_NAME = 'Sales'
	)
);
```

d. Find the last names and salaries of those employees whose salary is within ± 5k of the
average salary of SALES department

```js
SELECT LAST_NAME, SALARY
FROM employees
WHERE ABS(SALARY - (
	SELECT AVG(SALARY)
	FROM employees
	WHERE DEPARTMENT_ID = (
		SELECT DEPARTMENT_ID
		FROM departments
		WHERE DEPARTMENT_NAME = 'Sales'
	)
) <= 5000;
```

### 6.2: Advanced Sub-query

Example 1. Retrieve those employees whose salary is higher than at least three other employees.

```js
SELECT EMPLOYEE_ID, LAST_NAME, SALARY
FROM employees E1
WHERE 3 <= (
	SELECT COUNT(*)
	FROM employees E2
	WHERE E2.SALARY < E1.SALARY
); 
```

Example 2. Find those departments which have at least one employee having JOB_ID as ‘IT_PROG’.

```js
SELECT DEPARTMENT_NAME
FROM departments D
WHERE EXISTS (
	SELECT *
	FROM employees E
	WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID AND E.JOB_ID = 'IT_PROG'
); 
```

Example 3. Show the name of the employees who get highest salary in their department, along with department name.

```js
SELECT EMPLOYEE_ID, LAST_NAME, SALARY, (
	SELECT DEPARTMENT_NAME
	FROM departments D
	WHERE E1.DEPARTMENT_ID = D.DEPARTMENT_ID
) "Department Name"															
FROM employees E1
WHERE SALARY = (
	SELECT MAX(SALARY)
	FROM employees E2
	WHERE E2.DEPARTMENT_ID = E1.DEPARTMENT_ID
); 
```

Example 4.  Show the last name and salary of each employee along with the
minimum salary and maximum salary of his/her department. 

```js
SELECT E.LAST_NAME, E.SALARY, D.MINSAL, D.MAXSAL
FROM EMPLOYEES E, (
	SELECT DEPARTMENT_ID AS DEPT, MIN(SALARY) MINSAL, MAX(SALARY) MAXSAL
	FROM EMPLOYEES
	GROUP BY DEPARTMENT_ID
) D
WHERE (E.DEPARTMENT_ID = D.DEPT)
ORDER BY E.SALARY ;
```

#### Exercise

b. Find those departments whose average salary is greater than the minimum salary of all other
departments. Print department names. Use sub-query. You can use join in the sub-queries.

```js
SELECT D1.DEPARTMENT_NAME, D2.AVG_SALARY 
FROM departments D1, (
	SELECT E1.DEPARTMENT_ID DEPT_ID, AVG(SALARY) AVG_SALARY
	FROM employees E1
	GROUP BY E1.DEPARTMENT_ID
) D2
WHERE D1.DEPARTMENT_ID = D2.DEPT_ID
AND D2.AVG_SALARY > ANY (
	SELECT SALARY
	FROM employees E2
);		
```

c. Find those department names which have the highest number of employees in service. Print
department names. Use sub-query. You can use join in the sub-queries.

```js
SELECT D1.DEPARTMENT_NAME, D2.EMP_COUNT
FROM departments D1, (
	SELECT E1.DEPARTMENT_ID DEPT_ID, COUNT(DISTINCT E1.EMPLOYEE_ID) EMP_COUNT
	FROM employees E1
	GROUP BY E1.DEPARTMENT_ID
) D2
WHERE D1.DEPARTMENT_ID = D2.DEPT_ID
AND D2.EMP_COUNT > ALL (
	SELECT COUNT(DISTINCT E2.EMPLOYEE_ID)
	FROM employees E2
	WHERE E2.DEPARTMENT_ID <> D1.DEPARTMENT_ID
	GROUP BY E2.DEPARTMENT_ID
);	
```

An optimized version which uses join in main query:

```js
SELECT 
    E1.DEPARTMENT_ID, 
    D.DEPARTMENT_NAME
FROM 
    EMPLOYEES E1
JOIN 
    DEPARTMENTS D ON E1.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE 
    (
        SELECT COUNT(*)
        FROM EMPLOYEES E2
        WHERE E2.DEPARTMENT_ID = E1.DEPARTMENT_ID
    ) = 
    (
        SELECT MAX(CNT)
        FROM (SELECT DEPARTMENT_ID, COUNT(*) CNT FROM EMPLOYEES GROUP BY DEPARTMENT_ID) D_COUNT
    )
GROUP BY 
    E1.DEPARTMENT_ID, D.DEPARTMENT_NAME;

```

d. Find those employees who worked in more than one department in the company. Print
employee last names. You cannot use join in the main query. Use sub-query. You can use join
in the sub-queries.

```js
SELECT EMPLOYEE_ID
FROM JOB_HISTORY J1
WHERE 1 < (
	SELECT COUNT(DISTINCT DEPARTMENT_ID)
	FROM JOB_HISTORY J2
	WHERE J1.EMPLOYEE_ID = J2.EMPLOYEE_ID
);
```

f. For each job type, find the employee who gets the highest salary. Print job title and last name
of the employee. Assume that there is one and only one such employee for every job type.

```sql
SELECT 
    E.JOB_ID, 
    J.JOB_TITLE,
    E.LAST_NAME,
    MAX(E.SALARY) AS MAX_SALARY
FROM 
    EMPLOYEES E
INNER JOIN 
    JOBS J ON E.JOB_ID = J.JOB_ID
GROUP BY 
    E.JOB_ID, J.JOB_TITLE, E.LAST_NAME
ORDER BY 
    E.JOB_ID;
```












