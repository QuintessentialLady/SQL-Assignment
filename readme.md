# README: SQL Solutions for Employee and Department Queries

### Query 1: List all employees whose salary is between 1,000 (exclusive) and 2,000 (exclusive). The output includes the employee name, department, and salary.
For this, i needed to use range to specify the two conditions of between 1000 and 2000
SELECT EmployeeName, Department, Salary
FROM Employees
WHERE Salary > 1000 AND Salary < 2000;
The idea was to the usethe `WHERE` for conditional clause.
Alternative Could have started with the first range or command which is salary select salary that is above 1000 and the second command would be to select salary that is below 2000 and extract  the ones that fit the criteria. If the salary data could have included nulls, we can filter that out.


### Query 2:Count the number of employees in department 30 who have both a salary and a commission.
SELECT COUNT(*) AS EmployeeCount
FROM Employees
WHERE Department = 30 AND Salary IS NOT NULL AND Commission IS NOT NULL;
```
This idea ensures that employees meet all three conditions: belong to department 30, have a non-null salary, and have a non-null commission.
Alternative can be If commission values could be zero and this is considered "having a commission," the condition `Commission IS NOT NULL` could be modified to `Commission >= 0`. This would be useful as an initial entry, i added 0 to those not having any commission, but changed it as it kept throwing errors.


### Query 3: Employees with Salary >= 1,000 and Living in Dallas
Find the name and salary of employees earning at least 1,000 and residing in Dallas.
SELECT EmployeeName, Salary
FROM Employees
WHERE Salary >= 1000 AND City = 'Dallas';
This combines two filters on salary and city using the `AND` operator.
Alternative Approach: For case-insensitive city matches, the condition could be modified to use `LOWER(City) = 'dallas'`.

---

### Query 4: Departments with No Current Employees
To Identify departments that currently have no employees assigned.
SELECT DepartmentID
FROM Departments
WHERE DepartmentID NOT IN (SELECT DISTINCT Department FROM Employees);
This uses a subquery to find department IDs present in the `Employees` table and excludes them using `NOT IN`.
Alternative Approach can be A `LEFT JOIN` could be used to achieve the same result, filtering out departments with matching employees:
SELECT d.DepartmentID
FROM Departments d
LEFT JOIN Employees e ON d.DepartmentID = e.Department
WHERE e.Department IS NULL;

### Query 5: Department Statistics (Average Salary and Employee Count)
List the department number, average salary, and total employee count for each department.

SELECT Department, AVG(Salary) AS AverageSalary, COUNT(*) AS EmployeeCount
FROM Employees
GROUP BY Department;

The query groups employees by department and calculates the average salary and count of employees for each group.
Alternative is to include departments with no employees, an outer join with the `Departments` table could be used:
SELECT d.DepartmentID, AVG(e.Salary) AS AverageSalary, COUNT(e.EmployeeID) AS EmployeeCount
FROM Departments d
LEFT JOIN Employees e ON d.DepartmentID = e.Department
GROUP BY d.DepartmentID;