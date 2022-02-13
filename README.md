# Pewlett-Hackard-Analysis
## Overview of the analysis: 
We are helping Bobby by using Postgres to create a database, and pgAdmin to work with the data he'll be importing. This project is builting relationship between different csv files, then we can analysis the data to confirm how PH can deal with the retirment issue.

Resources
Data: 
departments.csv;
dept_emp.csv;
dept_manager.csv;
employees.csv;
salaries.csv;
titles.csv

Software: PostgreSQL and pgAdmin

## Results: 

First analysis:

* Confirming the number of retirement-age employees, who is current working at company.
* Identify the most recent title of retiring people, this can help company know how to plan the people in  mentorship program.

<img width="137" alt="retiring_titles" src="https://user-images.githubusercontent.com/95401877/153555090-de5a9afc-459d-42bf-adf7-0735c8e65126.png">

Second analysis:

* Confirming the number of retiring people who is mentorship-eligibility by their birthday.
* Preparing to clearfy the title of mentorship-eligibility people for further needs.

<img width="121" alt="mentorship_eligibilty_count" src="https://user-images.githubusercontent.com/95401877/153555075-ad67a4ea-cf92-43b1-9c6b-f8cb77f45a08.png">

## Summary: 

To check how many roles will need to be filled as the "silver tsunami" begins to make an impact, we should know how many people are retiring. Then we will know how many people will be hired. We has the total number about 13000. 


[retirement_titles_count.csv](https://github.com/Ammonwyz/Pewlett-Hackard-Analysis/files/8055039/retirement_titles_count.csv)

The ratio of new comers and mentorship_eligibilty employees is around 8:1 now, this is not good. Therefore, there are not enought qualitied. If we want 3:1 to 5:1 ratio, the company needs 1100 to 2800 more mentors.

Also, we can check the eligible people with departments, to confirm the program details.

Current employees in each department as show below:

<img width="97" alt="department_breakdown" src="https://user-images.githubusercontent.com/95401877/153740518-925cb643-da91-4eeb-a511-fe4f84a800f7.png">

Eligible for mentorship employees in each department as show below:

SELECT COUNT(me.emp_no), de.dept_no, d.dept_name

INTO mentorship_eligibilty_department_breakdown

FROM mentorship_eligibilty as me

LEFT JOIN dept_emp as de

ON me.emp_no = de.emp_no

LEFT JOIN departments as d

ON de.dept_no = d.dept_no

GROUP BY de.dept_no, d.dept_no

ORDER BY de.dept_no;

SELECT * FROM mentorship_eligibilty_department_breakdown;

<img width="198" alt="mentorship_eligibilty_department_breakdown" src="https://user-images.githubusercontent.com/95401877/153740468-0a351eb3-8643-4861-b1e3-98681020a036.png">

To make the program works, we can get a bigger extent, such as include employees who was born in 1964. Now, we have enough qualitied.

SELECT DISTINCT ON (e.emp_no) e.emp_no,

        e.first_name,
	
        e.last_name,
	
        e.birth_date,
	
	de.from_date,
	
        de.to_date,
	
	tl.title
	
INTO mentorship_eligibilty_extension

FROM employees AS e

INNER JOIN dept_emp AS de

ON (e.emp_no = de.emp_no)

INNER JOIN titles AS tl

ON (e.emp_no = tl.emp_no)

WHERE de.to_date = ('9999-01-01')

AND (e.birth_date BETWEEN '1964-01-01' AND '1964-12-31')

ORDER BY emp_no

select * from mentorship_eligibilty_extension

SELECT COUNT(mee.title),mee.title

INTO mentorship_eligibilty_extension_count

FROM mentorship_eligibilty_extension AS mee

GROUP BY mee.title

ORDER BY count(mee.title) DESC

SELECT * FROM mentorship_eligibilty_extension_count

<img width="137" alt="mentorship_eligibilty_extenstion_count" src="https://user-images.githubusercontent.com/95401877/153740460-94e13a79-3d1d-43f4-a273-137a3404e591.png">

