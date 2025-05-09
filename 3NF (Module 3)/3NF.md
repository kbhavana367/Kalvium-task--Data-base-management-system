Ever felt like your database is still a bit messy, even after cleaning it up with 1NF and 2NF? Imagine you've organized your clothes into shirts, pants, and accessories, but now you notice some items are in the wrong sections. That’s where Third Normal Form (3NF) comes in!


Learning Objectives

In this lesson, you'll learn:



The concept of Third Normal Form (3NF) and why it's important.

How to identify transitive dependencies in a database table.

Steps to apply 3NF to eliminate redundancy and improve data consistency using SQLite examples.

How to decompose tables to meet 3NF criteria.

Practical implementation of 3NF using SQLite.



Think about a scenario where you have a customer's address in your order details. The address depends on the customer ID, not directly on the order itself. This is similar to what 3NF addresses. It ensures that non-key attributes depend directly on the primary key and not on other non-key attributes.


Understanding Third Normal Form (3NF)

Third Normal Form (3NF) builds upon 2NF by addressing transitive dependencies. A table is in 3NF if it meets these conditions:



It is already in 2NF (no partial dependencies).

No non-key attribute is dependent on another non-key attribute.


   Transitive dependency means that a non-key attribute depends on another non-key attribute, which in turn depends on the primary key. If this happens, the table is not in 3NF.


Identifying Transitive Dependencies

Let’s say we have a table called Employees with the following structure:


Employees (EmployeeID, EmployeeName, DepartmentID, DepartmentName)


Here, EmployeeID is the primary key. DepartmentName depends on DepartmentID, which in turn depends on EmployeeID. This is a transitive dependency because EmployeeName -> DepartmentID -> DepartmentName.


To identify transitive dependencies:



Determine the primary key of the table.

List all non-key attributes.

For each non-key attribute, determine if it depends on the primary key directly or through another non-key attribute.


Why is 3NF Important?

Consider the consequences if we don't address transitive dependencies:



Redundancy: DepartmentName will be repeated for every employee in that department.

Update Anomalies: If we need to update a DepartmentName, we must update it in every row where it appears, leading to potential inconsistencies.

Insertion Anomalies: We cannot add a new department without associating it with an employee.

Deletion Anomalies: If we delete the only employee in a specific department, we lose the department information.


3NF helps prevent these issues by ensuring that each attribute is stored only once, reducing redundancy and improving data consistency.


Applying 3NF: Eliminating Transitive Dependencies

To achieve 3NF, we need to decompose the table to remove transitive dependencies. Let’s break down the Employees table into two tables:



Employees (EmployeeID, EmployeeName, DepartmentID)

Departments (DepartmentID, DepartmentName)


Now, Employees contains information about the employees, and Departments contains information about the departments. DepartmentName now depends only on DepartmentID in the Departments table, and all attributes in Employees depend directly on the primary key (EmployeeID).


Practical Implementation with SQLite

Let's create these tables in SQLite:


CREATE TABLE Departments (
    DepartmentID INTEGER PRIMARY KEY,
    DepartmentName TEXT
);

CREATE TABLE Employees (
    EmployeeID INTEGER PRIMARY KEY,
    EmployeeName TEXT,
    DepartmentID INTEGER,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

Now, let’s insert some sample data:


INSERT INTO Departments (DepartmentID, DepartmentName) VALUES
(1, 'Sales'),
(2, 'Marketing'),
(3, 'Engineering');

INSERT INTO Employees (EmployeeID, EmployeeName, DepartmentID) VALUES
(101, 'Alice', 1),
(102, 'Bob', 2),
(103, 'Charlie', 1),
(104, 'David', 3);

With this structure, updating a department name only requires changing it in the Departments table, avoiding inconsistencies.


Benefits of Applying 3NF

By applying 3NF, we achieve:



Reduced Redundancy: DepartmentName is stored only once in the Departments table.

Improved Data Consistency: Updating DepartmentName is straightforward and avoids inconsistencies.

Simplified Queries: Easier to manage and query data.

Efficient Storage: Less storage space is required due to reduced redundancy.



Imagine you're organizing your bookshelf. You group books by genre, and then you have a separate list of authors and their biographies. 3NF is like having that separate list of authors, making sure you don't repeat author information every time you list a book.


Considerations and Trade-offs

While 3NF improves database design, consider these trade-offs:



Increased Complexity: Decomposing tables can increase the number of tables, adding complexity to the database schema.

More Joins: Queries might require more JOIN operations to retrieve data from multiple tables.


However, the benefits of reduced redundancy and improved data consistency usually outweigh these costs.


Common Pitfalls and How to Avoid Them


Incorrectly Identifying Dependencies: Ensure dependencies are correctly identified. Misidentifying dependencies can lead to incorrect decomposition.

Over-Normalization: Avoid excessive normalization, which can lead to too many tables and complex queries.

Ignoring Business Requirements: Always consider the specific needs of the application and business rules when normalizing.


Real-World Examples


University Database: In a university database, course details (name, credits, department) should be stored in a separate Courses table, while student details (student ID, name, major) are stored in a Students table.

Hospital Management System: Doctor details (doctor ID, name, specialization) should be stored in a Doctors table, and appointment details (appointment ID, patient ID, doctor ID) are stored in an Appointments table.


Summary

In this lesson, you've learned about Third Normal Form (3NF), its importance in eliminating transitive dependencies, and how to apply it using SQLite. By decomposing tables and ensuring that non-key attributes are directly dependent on the primary key, you can reduce redundancy and improve data consistency in your databases. Now, think about a database you've worked with. Can you identify any tables that might benefit from being in 3NF?


Additional Resources for you:



https://www.guru99.com/third-normal-form.html