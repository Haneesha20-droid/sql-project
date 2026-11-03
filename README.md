CREATE TABLE employee (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    department VARCHAR(50),
    salary INT,
    city VARCHAR(50)
);

INSERT INTO employee VALUES
(1, 'Rahul', 'IT', 60000, 'Hyderabad'),
(2, 'Priya', 'HR', 45000, 'Bangalore'),
(3, 'Arun', 'IT', 75000, 'Hyderabad'),
(4, 'Sneha', 'Finance', 55000, 'Chennai'),
(5, 'Kiran', 'HR', 50000, 'Hyderabad'),
(6, 'Divya', 'IT', 75000, 'Bangalore'),
(7, 'Manoj', 'Finance', 65000, 'Chennai'),
(8, 'Anjali', 'HR', 40000, 'Bangalore'),
(9, 'Vikram', 'IT', 50000, 'Chennai'),
(10, 'Pooja', 'Finance', 55000, 'Hyderabad'),
(11, 'Ravi', 'Sales', 35000, 'Bangalore'),
(12, 'Meena', 'Sales', 48000, 'Hyderabad');

-- CTE 1: Employees earning more than 50000
WITH high_salary AS (
    SELECT *
    FROM employee
    WHERE salary > 50000
)
SELECT * FROM high_salary;

-- CTE 2: Average salary
WITH avg_salary AS (
    SELECT AVG(salary) AS average_salary
    FROM employee
)
SELECT *
FROM employee
WHERE salary > (SELECT average_salary FROM avg_salary);

-- CTE 3: Highest-paid employee in each department
WITH emp_rank AS (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY department
               ORDER BY salary DESC
           ) AS rn
    FROM employee
)
SELECT *
FROM emp_rank
WHERE rn = 1;

-- CTE 4: Rank employees by salary
WITH salary_rank AS (
    SELECT *,
           DENSE_RANK() OVER (
               ORDER BY salary DESC
           ) AS rnk
    FROM employee
)
SELECT * FROM salary_rank;

-- CTE 5: Department-wise average salary
WITH dept_avg AS (
    SELECT department,
           AVG(salary) AS average_salary
    FROM employee
    GROUP BY department
)
SELECT * FROM dept_avg;