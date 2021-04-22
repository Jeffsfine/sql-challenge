# EmployeeSQL
## Background
"It is a beautiful spring day, and it is two weeks since you have been hired as a new data engineer at Pewlett Hackard. Your first major task is a research project on employees of the corporation from the 1980s and 1990s. All that remain of the database of employees from that period are six CSV files."

With this challenge, I designed tables to hold data in CSVs, imported the CSVs into a SQL database, and answered questions about the data.
List the following details of each employee: employee number, last name, first name, sex, and salary.


- List first name, last name, and hire date for employees who were hired in 1986.
- List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.
- List the department of each employee with the following information: employee number, last name, first name, and department name.
- List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."
- List all employees in the Sales department, including their employee number, last name, first name, and department name.
- List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.
- In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.

## Data Modeling

The employee data was modeled with an Entity-Relationship Diagram (ERD) to start. This technique provides a clear visual structure of the relationship between the different datasets used in .csv format.

ERD Pictured:

![Employees_db_ERD](https://user-images.githubusercontent.com/74028387/115636941-117fea80-a2dd-11eb-891b-a08a9b9a2be8.png)

## Data Engineering

The ERD aided creation of table schemas for each category: `employees`, `departments`, `salaries`, `titles`, `department managers`, and `department employees`.

Data types, primary keys, foreign keys, and other constraints are listed in-line.
 
### Departments
``` CREATE TABLE "departments" (
    -- Department number is a primary key, and it is
    -- aslo found in department employees and department manager list
    "dept_no" VARCHAR   NOT NULL,
    -- Department names
    "dept_name" VARCHAR   NOT NULL,
    CONSTRAINT "pk_departments" PRIMARY KEY (
        "dept_no"
     )
);
```
### Ttiles
```CREATE TABLE "titles" (
    -- Title id is a primary key,
    -- and it also found in employees as emp_title_id
    "title_id" VARCHAR   NOT NULL,
    -- List of titles
    "title" VARCHAR   NOT NULL,
    CONSTRAINT "pk_titles" PRIMARY KEY (
        "title_id"
     )
);
```
### Employees
```CREATE TABLE "employees" (
    -- Employees number is a primary key
    -- and also found in department employees, department manager
    -- and salaries list
    "emp_no" INT   NOT NULL,
    -- Employees have a title id employees(emp_title_id)
    -- So, this id has relationship with-
    -- the composite foreign key titles(title_id)
    "emp_title_id" VARCHAR   NOT NULL,
    -- Employees birth date
    "birth_date" DATE   NOT NULL,
    -- Employees first name
    "first_name" VARCHAR   NOT NULL,
    -- Employees last name
    "last_name" VARCHAR   NOT NULL,
    -- Employees sex
    "sex" VARCHAR   NOT NULL,
    -- Employees hired date
    "hire_date" DATE   NOT NULL,
    CONSTRAINT "pk_employees" PRIMARY KEY (
        "emp_no"
     )
);
```
### Department Employees
```CREATE TABLE "dept_emp" (
    -- Employees number in department employees list and
    -- which shared a unique key with employees(emp_no)
    "emp_no" INT   NOT NULL,
    -- Department number in department employees list and
    -- which shared a unique key with dept_emp(dept_no)
    "dept_no" VARCHAR   NOT NULL
);
```
### Department Manager
```CREATE TABLE "dept_manager" (
    -- Department number in department manger list and
    -- which shared a unique key with dept_emp(dept_no)
    "dept_no" VARCHAR   NOT NULL,
    -- Employees number in department manger list and
    -- which  shared a unique key with employees(emp_no)
    "emp_no" INT   NOT NULL
);
```
### Salaries
```CREATE TABLE "salaries" (
    -- Employees number in salaries and
    -- which shared unique keys with employees(emp_no)
    "emp_no" INT   NOT NULL,
    -- Employees salaries
    "salary" INT   NOT NULL
);
```
## Data Analysis
- List the following details of each employee: employee number, last name, first name, sex, and salary.
```SELECT employees.emp_no, employees.last_name, employees.first_name, employees.sex, salaries.salary
FROM employees
JOIN salaries
ON employees.emp_no = salaries.emp_no;
```
- List first name, last name, and hire date for employees who were hired in 1986.
```SELECT first_name, last_name, hire_date 
FROM employees
WHERE hire_date BETWEEN '1986-01-01' AND '1987-01-01';
```
- List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.
```SELECT departments.dept_no, departments.dept_name, dept_manager.emp_no, employees.last_name, employees.first_name
FROM departments
JOIN dept_manager
ON departments.dept_no = dept_manager.dept_no
JOIN employees
ON dept_manager.emp_no = employees.emp_no;
```
- List the department of each employee with the following information: employee number, last name, first name, and department name.
```SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON dept_emp.dept_no = departments.dept_no;
```
- List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."
SELECT first_name, last_name,sex
FROM employees
WHERE first_name = 'Hercules'
AND last_name LIKE 'B%';
```
- List all employees in the Sales department, including their employee number, last name, first name, and department name.
```SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON dept_emp.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales';
```
- List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.
```SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON dept_emp.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales' 
OR departments.dept_name = 'Development';
```
- In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.
```SELECT last_name,
COUNT(last_name) AS "frequency"
FROM employees
GROUP BY last_name
ORDER BY
COUNT(last_name) DESC;
```
