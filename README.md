# Pewlett-Hackard-Analysis


## Overview of the analysis :

In this project we used the ERD, Employee Database with PostgreSQL and knowledge of SQL queries to get the required information: 
1. Determine the number of employees retiring per title
2. Identify employees who are eligible to participate in a mentorship program.

## Results


#### DELIVERABLE 1 
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
     SELECT DISTINCT ON (rt.emp_no) rt.emp_no,
     rt.first_name,
     rt.last_name,
     rt.title
     INTO unique_titles
     FROM retirement_titles as rt
     ORDER BY emp_no, to_date DESC;
     SELECT * from unique_titles;

 - ![unique_titles.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/unique_titles.PNG)
 
##### Unique_titles
     SELECT COUNT(ut.emp_no), ut.title
     INTO retiring_titles
     FROM unique_titles as ut
     GROUP BY ut.title
     ORDER BY COUNT (ut.emp_no) DESC;
     SELECT * from retiring_titles

 - ![retiring_titles.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/retiring_titles.PNG)

#### DELIVERABLE 2 
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

 - ![mentorship_eligibility.PNG](https://github.com/tjavaheripour/Pewlett-Hackard-Analysis/blob/main/Resources/mentorship_eligibility.PNG)


## Summary


 
