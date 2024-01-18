## Solution

## Chapter 3

### 3.3

a. For all employees, find the number of years employed. Print first names and number of years
employed for each employee.

```
SELECT FIRST_NAME, MONTHS_BETWEEN((SYSDATE – HIRE_DATE)) / 12 “Years employed”
FROM employees;

```

b. Suppose you need to find the number of days each employee worked during the first month
of his joining. Write an SQL query to find this information for all employees

```
SELECT FIRST_NAME, 30 - EXTRACT(DAY FROM HIRE_DATE) "Days Worked in First Month"
FROM employees;

```

### 3.4

a. Print the commission_pct values of all employees whose commission is at least 20%. Use NVL function.

```
SELECT FIRST_NAME, COMISSION_PCT
FROM employees
WHERE NVL(COMISSION_PCT, 0) >= 0.2;

```

b. Print the total salary of an employee for 5 years and 6 months period. Print all employee last
names along with this salary information. Use NVL function assuming that salary may
contain NULL values

```
SELECT FIRST_NAME, (SALARY * 66 + SALARY * 66 * NVL(COMISSION_PCT, 0)) "Salary"
FROM employees;
```

### Note: Converting date

Expression                               Output
TO_CHAR(HIRE_DATE, ‘DD/MM/YYYY’)         ‘31/01/1995’
TO_CHAR(HIRE_DATE, ‘MONTH DD, YYYY’)     ‘JANUARY 31, 1995’
TO_CHAR(HIRE_DATE, ‘MONTH DD, YEAR’)     ‘JANUARY 31, NINETEEN HUNDRED AND NINETY FIVE'
TO_CHAR(HIRE_DATE, ‘Ddspth Month, YEAR’) ‘Thirty-First January, Nineteen hundred and ninety five’
TO_CHAR(123456)                          ‘123456’

### 3.5

Example. Finds all employee last names who was hired before 1st January 1997.

```
SELECT LAST_NAME, TO_CHAR(HIRE_DATE, 'DD-MON-YYYY') "Hire Date"
FROM employees
WHERE HIRE_DATE < TO_DATE('01-JAN-1997', 'DD-MON-YYYY')
ORDER BY HIRE_DATE ASC;
```

a. Print hire dates of all employees in the following formats:
(i) 13th February, 1998 (ii) 13 February, 1998.

i.
```
SELECT LAST_NAME, TO_CHAR(HIRE_DATE,'ddth Month, YYYY') "Hire Date"
FROM employees
ORDER BY HIRE_DATE ASC;
```

ii.
```
SELECT LAST_NAME, TO_CHAR(HIRE_DATE,'dd Month, YYYY') "Hire Date"
FROM employees
ORDER BY HIRE_DATE ASC;
```


