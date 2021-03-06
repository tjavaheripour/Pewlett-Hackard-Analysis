# Pewlett-Hackard-Analysis


## Overview of the analysis :

In this project we used [ this ERD ](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/EmployeeDB.png), Employee Database with PostgreSQL and knowledge of SQL queries to get the required information: 
1. Determine the number of employees retiring per title
2. Identify employees who are eligible to participate in a mentorship program.

## Results

#### DELIVERABLE 1 : The Number of Retiring Employees by Title

Using [ this ERD ](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/EmployeeDB.png) to write a query that create a Retirement Titles table which holds all the titles of current employees who were born between January 1, 1952 and December 31, 1955. 

- In the first step, the table holds all the titles of current employees which are 133,776 employee.
##### Export retirement_titles.csv
     SELECT e.emp_no,
     e.first_name,
     e.last_name,
     t.title,
     t.from_date,
     t.to_date
     INTO retirement_titles
     FROM employees as e
     INNER JOIN titles as t
     ON (e.emp_no = t.emp_no)
     WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
     ORDER BY e.emp_no;
     ELECT * FROM retirement_titles;

 - ![retirement_titles.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/retirement_titles.PNG)

##### Use Dictinct with Orderby to remove duplicate rows
There are duplicate entries for some employees because they have switched titles over the years. We created a query to remove these duplicates and keep only the most recent title of each employee.
- There are 90,398 employee who will be retiring soon.
##### Export unique_titles.csv
     SELECT DISTINCT ON (rt.emp_no) rt.emp_no,
     rt.first_name,
     rt.last_name,
     rt.title
     INTO unique_titles
     FROM retirement_titles as rt
     ORDER BY emp_no, to_date DESC;
     SELECT * from unique_titles;

 - ![unique_titles.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/unique_titles.PNG)
 
##### Retrieve the number of employees by their most recent job title who are about to retire
In this query, we used COUNT() to retrieve the number of titles and GROUP BY functions to group the table by title, then sort the count column in descending order.

##### Export retiring_titles.csv
     SELECT COUNT(ut.emp_no), ut.title
     INTO retiring_titles
     FROM unique_titles as ut
     GROUP BY ut.title
     ORDER BY COUNT (ut.emp_no) DESC;
     SELECT * from retiring_titles

  ![retiring_titles.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/retiring_titles.PNG)

#### DELIVERABLE 2 : The Employees Eligible for the Mentorship Program
We used [ this ERD ](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/EmployeeDB.png) to write a query and create a mentorship-eligibility table that holds the current employees who are eligible to particpate in Pewlett Hackard's mentorship program and were born between January 1, 1965 and December 31, 1965.
In this query, We used the DISTINCT ON statement to create a table that contains the most recent title of each employee and prevent duplicate employees who have held more than one title with Pewlett Hackard.Also to create this table, We join the Employees and the Titles tables on e.emp_no as primary key.
- There are 1549 employee eligible for the mentorship program

##### Export mentorship_eligibilty.csv 

     SELECT DISTINCT ON (e.emp_no) e.emp_no,
     e.first_name,
     e.last_name,
     e.birth_date,
     de.from_date,
     de.to_date,
     t.title
     INTO mentorship_eligibility
     FROM employees AS e
     INNER JOIN dept_emp AS de
     ON (e.emp_no = de.emp_no)
     INNER JOIN titles AS t
     ON (e.emp_no = t.emp_no)
     WHERE de.to_date = ('9999-01-01')
         AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
     ORDER BY emp_no;
     SELECT * from mentorship_eligibility

  ![mentorship_eligibility.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/mentorship_eligibility.PNG)


## Summary

- How many roles will need to be filled as the "silver tsunami" begins to make an impact?

      select count (ut.title)
      from unique_titles as ut
By running this query, we can see 90,398 potential positions will need to be filled as the "silver tsunami" begins to make an impact

- Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?

      select count (ut.title)
      from unique_titles as ut
 
There are about 1549 employees who can become mentors.
