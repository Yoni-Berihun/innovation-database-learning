

# 🧪 Exercises — SQL Basics

These exercises help you practice the four fundamental SQL operations: SELECT, INSERT, UPDATE, and DELETE. Work through them in pgAdmin, and don't worry about mistakes — that's how you learn.

---

## 🟢 Easy Exercises

### Exercise 6.1: Your First SELECT

**Task:** Run these queries in pgAdmin and write down what you see.

a) `SELECT 'I am learning SQL!';`

Result: ___________________

b) `SELECT 20 * 3;`

Result: ___________________

c) `SELECT current_date;`

Result: ___________________

---

### Exercise 6.2: Select from a Table

**Task:** Assume the `students` table exists with these columns: `student_id`, `first_name`, `last_name`, `email`, `gpa`, `enrollment_date`.

Write the SQL to:

a) Select all columns for all students.
### b) Select only first names and last names.
```sql
SELECT first_name, last_name FROM students;
```

### c) Select all students with a GPA above 3.5.
```sql
SELECT * FROM students WHERE gpa > 3.5;
```

---

### Exercise 6.3: Insert a New Student
**Task:** Write the SQL to add a new student with these details:
* student_id: 11
* first_name: 'Kidist'
* last_name: 'Assefa'
* email: 'kidist@email.com'
* gpa: 3.65
* enrollment_date: '2024-09-01'

```sql
INSERT INTO students (student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES (11, 'Kidist', 'Assefa', 'kidist@email.com', 3.65, '2024-09-01');
```

#### After inserting, how would you verify the student was added?
```sql
SELECT * FROM students WHERE student_id = 11;
```

---

### Exercise 6.4: Update a Record
**Task:** Write the SQL to change Kidist's email to 'kidist.assefa@university.edu'.
```sql
UPDATE students
SET email = 'kidist.assefa@university.edu'
WHERE student_id = 11;
```

#### What would happen if you forgot the WHERE clause?
> 🚨 **ANSWER:** If you forgot the `WHERE` clause, **EVERY single student** in the database would have their email changed to `'kidist.assefa@university.edu'`. This would overwrite all existing student emails and corrupt your data.

---

### Exercise 6.5: Delete Safely
**Task:** Write the SQL to remove the student with student_id = 11.
```sql
DELETE FROM students WHERE student_id = 11;
```

#### Before running DELETE, what query should you run to confirm you're deleting the right record?
```sql
SELECT * FROM students WHERE student_id = 11;
```

---

## 🟡 Medium Exercises

### Exercise 6.6: Insert Multiple Records
**Task:** Write a single INSERT statement to add three new departments:


| department_id | name | building | budget |
| :--- | :--- | :--- | :--- |
| 50 | Chemistry | Science Hall | 400000 |
| 60 | Biology | Science Hall | 380000 |
| 70 | History | Arts Building | 220000 |

```sql
INSERT INTO departments (department_id, name, building, budget)
VALUES 
(50, 'Chemistry', 'Science Hall', 400000),
(60, 'Biology', 'Science Hall', 380000),
(70, 'History', 'Arts Building', 220000);
```

#### After inserting, write a query to verify all three were added.
```sql
SELECT * FROM departments WHERE department_id IN (50, 60, 70);
```

---

### Exercise 6.7: Update Multiple Columns
**Task:** The Computer Science department moved to a new building and received a budget increase. Write the SQL to:
* Change the building to 'New Tech Center'
* Change the budget to 600000
* Only for department_id = 10

```sql
UPDATE departments
SET building = 'New Tech Center',
    budget = 600000
WHERE department_id = 10;
```

#### Write a query to confirm the changes.
```sql
SELECT * FROM departments WHERE department_id = 10;
```

---

### Exercise 6.8: Select with Conditions
**Task:** Write SQL queries for these requests:

#### a) Find all courses with exactly 4 credits.
```sql
SELECT * FROM courses WHERE credits = 4;
```

#### b) Find all departments in 'Science Hall'.
```sql
SELECT * FROM departments WHERE building = 'Science Hall';
```

#### c) Find all students enrolled on '2024-09-01'.
```sql
SELECT * FROM students WHERE enrollment_date = '2024-09-01';
```

---

### Exercise 6.9: Safe Update Practice
**Task:** You need to update the GPA of student_id = 3 from 3.20 to 3.30.

#### Step 1: Write a SELECT query to see the current record.
```sql
SELECT * FROM students WHERE student_id = 3;
```

#### Step 2: Write the UPDATE query.
```sql
UPDATE students
SET gpa = 3.30
WHERE student_id = 3;
```

#### Step 3: Write a SELECT query to confirm the change.
```sql
SELECT * FROM students WHERE student_id = 3;
```

---

### Exercise 6.10: Working with Courses
**Task:** The university added a new course. Write the SQL to insert it:
* course_id: 106
* course_code: 'CS150'
* title: 'Data Structures'
* credits: 4
* department_id: 10

```sql
INSERT INTO courses (course_id, course_code, title, credits, department_id)
VALUES (106, 'CS150', 'Data Structures', 4, 10);
```

#### Then write a query to find all courses in the Computer Science department (department_id = 10).
```sql
SELECT * FROM courses WHERE department_id = 10;
```

---

## 🔴 Challenging Exercises

### Exercise 6.11: University Registration Scenario
**Task:** Walk through a complete registration scenario using SQL.

#### Part A: A new student named Liya Tadesse (student_id = 12, email: liya@email.com, gpa: 3.55) enrolls on 2024-09-01. Write the INSERT.
```sql
INSERT INTO students (student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES (12, 'Liya', 'Tadesse', 'liya@email.com', 3.55, '2024-09-01');
```

#### Part B: The registrar realizes Liya's email was entered incorrectly. It should be 'liya.tadesse@university.edu'. Write the UPDATE.
```sql
UPDATE students
SET email = 'liya.tadesse@university.edu'
WHERE student_id = 12;
```

#### Part C: Liya decides to defer enrollment. Write the SQL to remove her record.
```sql
DELETE FROM students WHERE student_id = 12;
```

#### Part D: Write a single query to show all students currently enrolled (enrollment_date = '2024-09-01') with their first name, last name, and GPA.
```sql
SELECT first_name, last_name, gpa 
FROM students 
WHERE enrollment_date = '2024-09-01';
```

---

### Exercise 6.12: Department Management
**Task:** The university is reorganizing departments.

#### Part A: Insert a new department: Physics (department_id: 80, building: 'Science Hall', budget: 320000).
```sql
INSERT INTO departments (department_id, name, building, budget)
VALUES (80, 'Physics', 'Science Hall', 320000);
```

#### Part B: The History department's budget needs to increase by 50000. Write an UPDATE that adds 50000 to the current budget.
```sql
UPDATE departments
SET budget = budget + 50000
WHERE name = 'History';
```

#### Part C: Write a query to find all departments with a budget greater than 350000.
```sql
SELECT * FROM departments WHERE budget > 350000;
```

#### Part D: The Chemistry department (id 50) is being merged into Physics. Write SQL to delete the Chemistry department.
```sql
DELETE FROM departments WHERE department_id = 50;
```

#### Part E: Before deleting, what potential problem might occur if courses still reference department_id = 50?
> 🚨 **ANSWER:** If other tables (like `courses`) contain rows with `department_id = 50`, running this query will trigger a **Foreign Key Constraint Violation Error**, preventing deletion. If foreign key constraints aren't set up, it will create **"orphan rows"** in the courses table, meaning courses will point to a department ID that no longer exists, corrupting data integrity.

---

### Exercise 6.13: Data Integrity Challenge

#### Scenario A: You accidentally run `UPDATE students SET gpa = 2.0;` without a WHERE clause.
1. **What should you have done before running this command?**
   * Always write a `SELECT` version of the query first to see what rows are targeted.
   * Put the query inside a **Transaction** block (`BEGIN TRANSACTION;` ... `ROLLBACK;`) to preview changes safely before committing.
2. **What could you do now?**
   * Restore the table from the latest formal database **backup (SQL dump)**.
   * If point-in-time recovery (PITR) or transaction logs are enabled, roll back to the second before the bad query executed.

#### Scenario B: Delete all students who enrolled before 2024-01-01.

##### Step 1: Check what you'll delete.
```sql
SELECT * FROM students WHERE enrollment_date < '2024-01-01';
```

##### Step 2: Delete the records.
```sql
DELETE FROM students WHERE enrollment_date < '2024-01-01';
```

##### Step 3: Verify the deletion.
```sql
SELECT * FROM students WHERE enrollment_date < '2024-01-01';
```

#### Scenario C: Update every student's email domain from '@email.com' to '@university.edu'.
```sql
UPDATE students
SET email = REPLACE(email, '@email.com', '@university.edu');
```
* **Approach in words:** Since we want to modify everyone, we omit the `WHERE` clause intentionally. We use a built-in text processing function (`REPLACE`) to target the specific text string inside the `email` column, substituting the old domain text with the new official university extension.


```sql
___________________
