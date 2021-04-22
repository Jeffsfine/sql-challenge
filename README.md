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
