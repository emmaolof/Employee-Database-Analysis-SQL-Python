--Data Engineering

CREATE TABLE "employees" (
    "emp_no" INT   NOT NULL,
    "birth_date" DATE   NOT NULL,
    "first_name" VARCHAR   NOT NULL,
    "last_name" VARCHAR   NOT NULL,
    "gender" VARCHAR   NOT NULL,
    "hire_date" DATE   NOT NULL,
    CONSTRAINT "pk_employees" PRIMARY KEY (
        "emp_no"
     )
);

CREATE TABLE "dept_emp" (
    "emp_no" INT   NOT NULL,
    "dept_no" VARCHAR   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL,
	FOREIGN KEY ("emp_no") REFERENCES "employees" ("emp_no"),
	FOREIGN KEY ("dept_no") REFERENCES "departments" ("dept_no")
);


CREATE TABLE "dept_manager" (
    "dept_no" VARCHAR   NOT NULL,
    "emp_no" INT   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL,
	FOREIGN KEY ("emp_no") REFERENCES "employees" ("emp_no"),
	FOREIGN KEY ("dept_no") REFERENCES "departments" ("dept_no")
);


CREATE TABLE "salaries" (
    "emp_no" INT   NOT NULL,
    "salary" INT   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL,
	FOREIGN KEY ("emp_no") REFERENCES "employees" ("emp_no")
);


CREATE TABLE "titles" (
    "emp_no" INT   NOT NULL,
    "title" VARCHAR   NOT NULL,
    "from_date" DATE   NOT NULL,
    "to_date" DATE   NOT NULL,
	FOREIGN KEY ("emp_no") REFERENCES "employees" ("emp_no")
);


CREATE TABLE "departments" (
    "dept_no" VARCHAR   NOT NULL,
    "dept_name" VARCHAR   NOT NULL,
    CONSTRAINT "pk_departments" PRIMARY KEY (
        "dept_no"
     )
);


SELECT * FROM dept_emp;

-- Data Analysis
1 --List the following details of each employee: employee number, last name, first name, gender, and salary
select e.emp_no, last_name, first_name, gender, salary
from employees e
join salaries s
on e.emp_no = s.emp_no

2 --List employees who were hired in 1986.
select emp_no, first_name, last_name, hire_date
from employees
where DATE_PART('year', hire_date) = 1986
order by hire_date ASC

3 -- List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name, and start and end employment dates.
select dm.dept_no, d.dept_name, dm.emp_no, e.last_name, e.first_name, de.from_date, de.to_date 
from dept_manager dm
join departments d
    on dm.dept_no = d.dept_no
join employees e
    on dm.emp_no = e.emp_no
join dept_emp de
    on dm.emp_no = de.emp_no
	
4 -- List the department of each employee with the following information: employee number, last name, first name, and department name.
select e.emp_no, last_name, first_name, d.dept_name
from employees e
join dept_emp de
	on e.emp_no = de.emp_no
join departments d
	on de.dept_no = d.dept_no
	
5 -- List all employees whose first name is "Hercules" and last names begin with "B."
select first_name, last_name
from employees
where first_name = 'Hercules' and last_name like 'B%'

6 -- List all employees in the Sales department, including their employee number, last name, first name, and department name.
select e.emp_no, last_name, first_name, d.dept_name
from employees e
join dept_emp de
	on e.emp_no = de.emp_no
join departments d
	on de.dept_no = d.dept_no
where dept_name = 'Sales'

7 -- List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.
select e.emp_no, last_name, first_name, d.dept_name
from employees e
join dept_emp de
	on e.emp_no = de.emp_no
join departments d
	on de.dept_no = d.dept_no
where dept_name = 'Sales' or dept_name = 'Development'

8 -- In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.
select last_name, count(last_name) count
from employees
group by last_name
order by count desc
