Ever felt like your data is a messy room, with everything scattered around? Just like you organize your room to find things easily, databases need organizing too. This is where normalization comes in, and today, we're diving into the First Normal Form (1NF)!

In this lesson You'll learn:

*  Understanding what normalization is and why it's crucial for database efficiency.
*  Defining First Normal Form (1NF) and its core principles.
*  Identifying repeating groups in unnormalized data.
*  Applying 1NF to eliminate repeating groups and achieve atomic values.
*  Writing SQLite code to transform data into 1NF.

## What is Normalization and Why Do We Need It?

Think of normalization as tidying up your digital data closet. It's the process of organizing data in a database to reduce **redundancy** (duplication) and improve **data integrity** (accuracy and consistency). Just like a well-organized closet makes it easier to find your favorite shirt, a normalized database makes it easier to retrieve and manage information.

Normalization is essential because it helps:

*   Minimize data redundancy: Reducing the amount of storage space required.
*   Improve data integrity: Ensuring data is accurate and consistent.
*   Simplify data modification: Making it easier to update and maintain the database.
*   Enhance query performance: Speeding up data retrieval.

## Understanding First Normal Form (1NF)

Imagine you have a contact list where you store multiple phone numbers in a single field. That's like putting multiple items in one box without any separation! First Normal Form (1NF) is the initial step in organizing this data. It sets a basic rule: each column in a table should contain only **atomic** values. An atomic value is indivisible; it cannot be further broken down.

**Key Principles of 1NF**:

*   **Eliminate Repeating Groups**: Ensure that there are no repeating groups of data within a table.
*   **Atomic Values**: Each column should contain only atomic (indivisible) values.

## Identifying Repeating Groups

Let's consider a scenario: a table of students and their courses. Suppose you have a table named `Students` with columns like `StudentID`, `Name`, and `Courses`. If a student is enrolled in multiple courses, you might be tempted to store all course names in a single `Courses` column, separated by commas. This is a repeating group.

**Example of Unnormalized Data**:

| StudentID | Name    | Courses                  |
| :-------- | :------ | :----------------------- |
| 1         | John    | Math, Physics, Chemistry |
| 2         | Alice   | Biology, History         |

In this case, the `Courses` column contains repeating groups (multiple courses) for each student. This violates 1NF.

## Applying 1NF to Eliminate Repeating Groups

To normalize the above data into 1NF, we need to eliminate the repeating group. The strategy is to create a separate table for courses and link it to the `Students` table using a foreign key.

**Steps to Apply 1NF**:

1.  **Create a New Table for Courses**: Create a new table named `Courses` with columns `CourseID` and `CourseName`.
2.  **Link Tables Using Foreign Key**: Add a new table named `StudentCourses` to link `Students` and `Courses` tables.

**Normalized Data**:

**Students Table**:

| StudentID | Name    |
| :-------- | :------ |
| 1         | John    |
| 2         | Alice   |

**Courses Table**:

| CourseID | CourseName |
| :------- | :--------- |
| 1        | Math       |
| 2        | Physics    |
| 3        | Chemistry  |
| 4        | Biology    |
| 5        | History    |

**StudentCourses Table**:

| StudentID | CourseID |
| :-------- | :------- |
| 1         | 1        |
| 1         | 2        |
| 1         | 3        |
| 2         | 4        |
| 2         | 5        |

Now, each column contains only atomic values, and there are no repeating groups.

## SQLite Code for 1NF Normalization

Let's translate the above normalization steps into SQLite code.

1.  **Create Students Table**:

```sqlite
CREATE TABLE Students (
    StudentID INTEGER PRIMARY KEY,
    Name TEXT
);
```

2.  **Create Courses Table**:

```sqlite
CREATE TABLE Courses (
    CourseID INTEGER PRIMARY KEY,
    CourseName TEXT
);
```

3.  **Create StudentCourses Table**:

```sqlite
CREATE TABLE StudentCourses (
    StudentID INTEGER,
    CourseID INTEGER,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID),
    PRIMARY KEY (StudentID, CourseID)
);
```

4.  **Insert Data into Students Table**:

```sqlite
INSERT INTO Students (StudentID, Name) VALUES
(1, 'John'),
(2, 'Alice');
```

5.  **Insert Data into Courses Table**:

```sqlite
INSERT INTO Courses (CourseID, CourseName) VALUES
(1, 'Math'),
(2, 'Physics'),
(3, 'Chemistry'),
(4, 'Biology'),
(5, 'History');
```

6.  **Insert Data into StudentCourses Table**:

```sqlite
INSERT INTO StudentCourses (StudentID, CourseID) VALUES
(1, 1),  -- John takes Math
(1, 2),  -- John takes Physics
(1, 3),  -- John takes Chemistry
(2, 4),  -- Alice takes Biology
(2, 5);  -- Alice takes History
```

This SQLite code creates the necessary tables and populates them with data in 1NF. Each table has atomic values, and the relationships between students and courses are maintained through the `StudentCourses` table.

---

**Real-World Example**:

Consider an e-commerce platform where each order can contain multiple products. In an unnormalized design, you might store all product names in a single "Products" column. By applying 1NF, you would create separate tables for orders and products, linking them with an order_products table. This ensures that each product is stored atomically and that there are no repeating groups.

---

**Interactive Example**:

Let's consider a scenario where you have a table named "Employees" with columns like "EmployeeID", "Name", and "Skills". Suppose an employee can have multiple skills, and you are storing these skills as a comma-separated string in the "Skills" column.

Unnormalized Data:

| EmployeeID | Name   | Skills             |
| :--------- | :----- | :----------------- |
| 1          | John   | Java, Python       |
| 2          | Alice  | SQL, Database      |
| 3          | Bob    | C++, Algorithm     |

The "Skills" column contains repeating groups (multiple skills) for each employee, violating 1NF.

To normalize this data into 1NF, you need to eliminate the repeating group. The strategy is to create a separate table for skills and link it to the "Employees" table using a foreign key.

**Steps to Apply 1NF**:

1.  Create a New Table for Skills: Create a new table named "Skills" with columns "SkillID" and "SkillName".
2.  Link Tables Using Foreign Key: Add a new table named "EmployeeSkills" to link "Employees" and "Skills" tables.

Normalized Data:

**Employees Table**:

| EmployeeID | Name  |
| :--------- | :---- |
| 1          | John  |
| 2          | Alice |
| 3          | Bob   |

**Skills Table**:

| SkillID | SkillName  |
| :------ | :--------- |
| 1       | Java       |
| 2       | Python     |
| 3       | SQL        |
| 4       | Database   |
| 5       | C++        |
| 6       | Algorithm  |

**EmployeeSkills Table**:

| EmployeeID | SkillID |
| :--------- | :------ |
| 1          | 1       |
| 1          | 2       |
| 2          | 3       |
| 2          | 4       |
| 3          | 5       |
| 3          | 6       |

Now, each column contains only atomic values, and there are no repeating groups.

**SQLite Code for 1NF Normalization**:

1.  Create Employees Table:

```sqlite
CREATE TABLE Employees (
    EmployeeID INTEGER PRIMARY KEY,
    Name TEXT
);
```

2.  Create Skills Table:

```sqlite
CREATE TABLE Skills (
    SkillID INTEGER PRIMARY KEY,
    SkillName TEXT
);
```

3.  Create EmployeeSkills Table:

```sqlite
CREATE TABLE EmployeeSkills (
    EmployeeID INTEGER,
    SkillID INTEGER,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID),
    FOREIGN KEY (SkillID) REFERENCES Skills(SkillID),
    PRIMARY KEY (EmployeeID, SkillID)
);
```

4.  Insert Data into Employees Table:

```sqlite
INSERT INTO Employees (EmployeeID, Name) VALUES
(1, 'John'),
(2, 'Alice'),
(3, 'Bob');
```

5.  Insert Data into Skills Table:

```sqlite
INSERT INTO Skills (SkillID, SkillName) VALUES
(1, 'Java'),
(2, 'Python'),
(3, 'SQL'),
(4, 'Database'),
(5, 'C++'),
(6, 'Algorithm');
```

6.  Insert Data into EmployeeSkills Table:

```sqlite
INSERT INTO EmployeeSkills (EmployeeID, SkillID) VALUES
(1, 1),  -- John has Java
(1, 2),  -- John has Python
(2, 3),  -- Alice has SQL
(2, 4),  -- Alice has Database
(3, 5),  -- Bob has C++
(3, 6);  -- Bob has Algorithm
```

This SQLite code creates the necessary tables and populates them with data in 1NF. Each table has atomic values, and the relationships between employees and skills are maintained through the EmployeeSkills table.

## Summary

In this lesson, you've learned about normalization and the importance of First Normal Form (1NF). You discovered how to identify repeating groups and apply 1NF to eliminate them, ensuring each column contains only atomic values. By normalizing your databases, you reduce redundancy, improve data integrity, and enhance overall database performance.

Now that you've mastered 1NF, are you ready to take your data organization skills to the next level? What challenges might you face when dealing with more complex normalization rules?

Additional Resources for you:
https://www.datacamp.com/tutorial/first-normal-form