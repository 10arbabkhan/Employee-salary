# Employee-salary
## Overview
This project involves the creation and analysis of an employees database. 
It includes two tables:
1. Employee_demographics: Contains employee details such as ID, name, age, and gender.
2. Employee_salary: Includes job titles and corresponding salaries for employees.
The project provides insights into employee data through a series of SQL queries designed to answer key business questions.

## Key Findings
Employee Demographics:
The dataset includes 9 employees with attributes like name, age, and gender.
Gender distribution and age range were identified as key demographic insights.

## Salary Analysis:
Salaries vary by job title, with the highest being $65,000 for the Regional Manager position.
Salesmen form the majority among employees, with salaries ranging from $45,000 to $63,000.

## Integrated Insights:
Analysis of salary by demographic characteristics such as age and gender provided additional depth.
Queries highlighted potential areas for HR and salary optimization.

# Schemas
```sql create database employees;
CREATE TABLE employee_demographics (
    employee_id INT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT,
    gender VARCHAR(50)
); 

CREATE TABLE employee_salary (
    employee_id INT,
    job_title VARCHAR(50),
    salary INT
);

Insert into employee_demographics VALUES
(1001, 'Jim', 'Halpert', 30, 'Male'),
(1002, 'Pam', 'Beasley', 30, 'Female'),
(1003, 'Dwight', 'Schrute', 29, 'Male'),
(1004, 'Angela', 'Martin', 31, 'Female'),
(1005, 'Toby', 'Flenderson', 32, 'Male'),
(1006, 'Michael', 'Scott', 35, 'Male'),
(1007, 'Meredith', 'Palmer', 32, 'Female'),
(1008, 'Stanley', 'Hudson', 38, 'Male'),
(1009, 'Kevin', 'Malone', 31, 'Male')
;

Insert Into employee_salary VALUES
(1001, 'Salesman', 45000),
(1002, 'Receptionist', 36000),
(1003, 'Salesman', 63000),
(1004, 'Accountant', 47000),
(1005, 'HR', 50000),
(1006, 'Regional Manager', 65000),
(1007, 'Supplier Relations', 41000),
(1008, 'Salesman', 48000),
(1009, 'Accountant', 42000)
;
```

## Business Problems and Solutions
-- Q1 Retrieve the full name and salary of employees earning more than $50,000.
```sql
SELECT 
    CONCAT(first_name, ' ', last_name) full_name, salary
FROM
    employee_demographics
        JOIN
    employee_salary ON employee_demographics.employee_id = employee_salary.employee_id
WHERE
    salary > 50000;
```
-- Q2 Find the average salary of all employees.
```sql
SELECT 
    AVG(salary) avg_salary
FROM
    employee_salary;
```
-- 3. List all employees whose age is greater than 35 and have a salary greater than $45,000.
```sql
SELECT 
    *
FROM
    employee_demographics
        JOIN
    employee_salary ON employee_demographics.employee_id = employee_salary.employee_id
WHERE
    age > 35 AND salary > 45000;
 ```
   
-- 4. Count the total number of employees by gender.
```sql
SELECT DISTINCT
    gender, COUNT(*) total_count
FROM
    employee_demographics
GROUP BY gender
ORDER BY gender;
```

-- 5. Retrieve the job title with the highest average salary.
```sql
SELECT DISTINCT
    job_title, AVG(salary) avg_sal
FROM
    employee_salary
GROUP BY job_title
ORDER BY avg_sal DESC
LIMIT 1;
```

-- 6. Find the youngest employee in the company and their salary.
```sql
SELECT 
    *
FROM
    employee_demographics
        JOIN
    employee_salary ON employee_demographics.employee_id = employee_salary.employee_id
ORDER BY age ASC
LIMIT 1;
```

-- 7. Find the number of employees earning less than $40,000.
```sql
SELECT 
    COUNT(salary)
FROM
    employee_salary
WHERE
    salary < 40000;
```

-- 8. Find the total salary expenditure of the company.
```sql
SELECT 
    SUM(salary) total_expeniture
FROM
    employee_salary;
```
-- 9. Retrieve employees with job titles starting with 'Manager'.
```sql
SELECT 
    CONCAT(first_name, ' ', last_name) Full_name, job_title
FROM
    employee_demographics
        JOIN
    employee_salary ON employee_demographics.employee_id = employee_salary.employee_id
WHERE
    job_title LIKE '%manager'
;
```
-- 10. List employees who are older than the average age of all employees.
```sql
SELECT 
    CONCAT(first_name, ' ', last_name) full_name, age
FROM
    employee_demographics
WHERE
    age > (SELECT 
            AVG(age)
        FROM
            employee_demographics);
```
-- 11. Retrieve employees whose salary is below the median salary.
```sql
SELECT DISTINCT
    job_title, AVG(salary) avg_sal
FROM
    employee_salary
GROUP BY job_title
ORDER BY avg_sal DESC;
```

-- 12. Find employees who earn more than the average salary for their job title.
```sql
SELECT 
    *
FROM
    employee_salary
WHERE
    (SELECT 
            salary > AVG(salary) avg_sal
        FROM
            employee_salary)
GROUP BY avg_sal;

SELECT 
    CONCAT(ed.first_name, ' ', ed.last_name) AS full_name,
    es.job_title,
    es.salary
FROM
    employee_demographics ed
        JOIN
    employee_salary es ON ed.employee_id = es.employee_id
WHERE
    es.salary > (SELECT 
            AVG(es_inner.salary)
        FROM
            employee_salary es_inner
        WHERE
            es_inner.job_title = es.job_title);
```

-- 13. Retrieve the employee(s) with the highest salary in each job title.
```sql
SELECT 
    ed.employee_id,
    CONCAT(ed.first_name, ' ', ed.last_name) AS full_name,
    es.job_title,
    es.salary
FROM
    employee_demographics ed
        JOIN
    employee_salary es ON ed.employee_id = es.employee_id
WHERE
    es.salary = (SELECT 
            MAX(es_inner.salary)
        FROM
            employee_salary es_inner
        WHERE
            es_inner.job_title = es.job_title);
```
