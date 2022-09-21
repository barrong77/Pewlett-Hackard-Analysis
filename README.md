# Pewlett-Hackard-Analysis
Overview of Pewlett Hackard Analysis
Purpose
The purpose of this analysis is to:
Determine the amount of people retiring from Pewlett Hackard by providing a table labled employees per title.  Also, to identify employees who can participate in a mentorship program.
Retiring Employees by Title
See Retirement Titles table that contains the titles of employees between January 1, 1952 and December 31, 1955.  Below is the query used.
SELECT employees.emp_no,
	employees.first_name,
    employees.last_name,
	titles.title,
    titles.from_date,
    titles.to_date
INTO retirement_titles
FROM employees
LEFT JOIN titles
ON employees.emp_no = titles.emp_no
WHERE employees.birth_date BETWEEN '1952-01-01' AND '1955-12-31'
ORDER BY employees.emp_no
SELECT * FROM retirement_titles
I used the following query to remove duplicate rows using the DISTINCT ON statement and most recent title of each employee.
-- Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (retirement_titles.emp_no) retirement_titles.emp_no,
	retirement_titles.first_name,
    retirement_titles.last_name,
	retirement_titles.title
INTO unique_titles
FROM retirement_titles
ORDER BY retirement_titles.emp_no, retirement_titles.to_date DESC;
SELECT * FROM unique_titles
Using the COUNT function to create a Retiring Titles table which shows the retirement age employees by job title.
SELECT title, COUNT(*)
INTO retiring_titles
FROM unique_titles
GROUP BY title
ORDER BY 2 DESC
SELECT * FROM retiring_titles
Employees Eligible for Mentorship Program
This section created an error for me.  I kept trying to figure out the issue.  But I was finally able to create the Mentorship Eligibilty table that holds the employees who were born between January 1, 1965 and December 31, 1965 see query below:
SELECT DISTINCT ON (employees.emp_no) employees.emp_no,
	employees.first_name,
	employees.last_name,
	employees.birth_date,
	dept_emp.from_date,
	dept_emp.to_date,
	titles.title
INTO mentorship_eligibility
FROM employees
LEFT JOIN dept_emp
ON employees.emp_no = dept_emp.emp_no
LEFT JOIN titles
ON titles.emp_no = employees.emp_no
WHERE employees.birth_date BETWEEN '1965-01-01' AND '1965-12-31'
ORDER BY employees.emp_no
SELECT * FROM mentorship_eligibility
Results
A total of 41,381 employees are of retirement-age per the retirement information table.
But if you look at the retiring title table it says 90,398 is the count of people ready for retirement.
A total of 1,940 employees are eligible to be mentors in the mentorship program.
Most of the people getting ready to retire are engineers.

Summary
The company has about 300,000 employees with 90,000 people eligible for retirment, almost a 1/3 of the workforce.  There are not enough mentors to fill the roles once people begin to retire.  Overall, that is a nightmare for the company if they want to stay in business at its current state.
