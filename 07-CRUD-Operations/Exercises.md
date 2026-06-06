
# 🧪 Exercises — CRUD Operations in PostgreSQL

These exercises help you master Create, Read, Update, and Delete through hands-on practice. Work through them in pgAdmin using your University Management System database.

---

## 🟢 Easy Exercises

### Exercise 7.1: Create a Student

**Task:** Write the SQL to add a new student to the database.

**Details:**
* student_id: 11
* first_name: 'Kidist'
* last_name: 'Assefa'
* email: 'kidist@university.edu'
* gpa: 3.65
* enrollment_date: '2024-09-01'

* #### After inserting, how do you verify the student was added?
```sql
SELECT * FROM students WHERE student_id = 11;
```

---

### Exercise 7.2: Read All Students
**Task:** Write SQL to retrieve all students' first names, last names, and GPAs.
```sql
SELECT first_name, last_name, gpa FROM students;
```

#### Now modify your query to show only students with a GPA above 3.5.
```sql
SELECT first_name, last_name, gpa FROM students WHERE gpa > 3.5;
```

---

### Exercise 7.3: Update an Email
**Task:** Kidist changed her email to 'kidist.assefa@university.edu'. Write the UPDATE statement.
```sql
UPDATE students
SET email = 'kidist.assefa@university.edu'
WHERE student_id = 11;
```

#### Before running the update, what SELECT query should you run to confirm you're updating the right record?
```sql
SELECT * FROM students WHERE student_id = 11;
```

---

### Exercise 7.4: Delete a Record
**Task:** A student with student_id = 11 has withdrawn. Write the SQL to remove them.
```sql
DELETE FROM students WHERE student_id = 11;
```

#### What should you check before running this DELETE?
> 🚨 **ANSWER:** Before running this `DELETE`, you must check if the student has any existing records in the `enrollments` table. If they do, you must delete those enrollment records first to prevent a **Foreign Key Constraint Violation Error** or creating "orphan data".

---

### Exercise 7.5: Create Multiple Courses
**Task:** Insert three new courses in a single statement.



| course_id | course_code | title | credits | department_id |
| :--- | :--- | :--- | :--- | :--- |
| 107 | CS150 | Data Structures | 4 | 10 |
| 108 | MATH201 | Calculus II | 4 | 20 |
| 109 | ENG102 | Creative Writing | 3 | 30 |

```sql
INSERT INTO courses (course_id, course_code, title, credits, department_id)
VALUES 
(107, 'CS150', 'Data Structures', 4, 10),
(108, 'MATH201', 'Calculus II', 4, 20),
(109, 'ENG102', 'Creative Writing', 3, 30);
```

#### Verify all three were added:
```sql
SELECT * FROM courses WHERE course_id IN (107, 108, 109);
```

---

## 🟡 Medium Exercises

### Exercise 7.6: Conditional Updates
**Task:** The Computer Science department (department_id = 10) decided all their courses should be 4 credits. Write the UPDATE statement.
```sql
UPDATE courses
SET credits = 4
WHERE department_id = 10;
```

#### Before updating, check which courses will be affected:
```sql
SELECT * FROM courses WHERE department_id = 10;
```

#### After updating, verify the change:
```sql
SELECT * FROM courses WHERE department_id = 10;
```

---

### Exercise 7.7: Select with Multiple Conditions
**Task:** Write SQL for each request.

#### a) Find all students who enrolled on '2024-09-01' AND have a GPA above 3.5.
```sql
SELECT * FROM students 
WHERE enrollment_date = '2024-09-01' AND gpa > 3.5;
```

#### b) Find all courses with exactly 3 credits OR exactly 4 credits.
```sql
SELECT * FROM courses 
WHERE credits = 3 OR credits = 4;
-- Alternative optimized way: SELECT * FROM courses WHERE credits IN (3, 4);
```

#### c) Find all students whose last name starts with 'A' or 'B'. (Hint: Use LIKE.)
```sql
SELECT * FROM students 
WHERE last_name LIKE 'A%' OR last_name LIKE 'B%';
```

---

### Exercise 7.8: Safe Deletion Practice
**Task:** You need to delete the course with course_id = 109.

#### Step 1: Check what you're about to delete.
```sql
SELECT * FROM courses WHERE course_id = 109;
```

#### Step 2: Check if any enrollments reference this course.
```sql
SELECT * FROM enrollments WHERE course_id = 109;
```

#### Step 3: Delete the course.
```sql
DELETE FROM courses WHERE course_id = 109;
```

#### Step 4: Confirm it's gone.
```sql
SELECT * FROM courses WHERE course_id = 109;
```

---

### Exercise 7.9: Update Multiple Columns
**Task:** Student with student_id = 2 changed their email and improved their GPA.
* New email: 'birtukan.tadesse@university.edu'
* New GPA: 3.75

```sql
UPDATE students
SET email = 'birtukan.tadesse@university.edu',
    gpa = 3.75
WHERE student_id = 2;
```

#### Verify both changes:
```sql
SELECT * FROM students WHERE student_id = 2;
```

---

### Exercise 7.10: Read with Sorting
**Task:** Write SQL for each request.

#### a) List all students sorted by GPA from highest to lowest.
```sql
SELECT * FROM students 
ORDER BY gpa DESC;
```

#### b) List all courses sorted by credits (ascending), then by title (ascending).
```sql
SELECT * FROM courses 
ORDER BY credits ASC, title ASC;
```

#### c) List the top 3 students by GPA. (Hint: Use LIMIT.)
```sql
SELECT * FROM students 
ORDER BY gpa DESC 
LIMIT 3;
```

---

## 🔴 Challenging Exercises

### Exercise 7.11: Complete Registration Workflow
**Task:** Walk through a complete student registration scenario.

#### Part A: A new student named Liya Tadesse (student_id = 12, email: liya@university.edu, gpa: 3.80) enrolls on 2024-09-01. Write the INSERT.
```sql
INSERT INTO students (student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES (12, 'Liya', 'Tadesse', 'liya@university.edu', 3.80, '2024-09-01');
```

#### Part B: Liya enrolls in three courses: CS101 (101), CS201 (102), and MATH101 (103). Write the INSERT statements for the enrollments table.
```sql
INSERT INTO enrollments (student_id, course_id, enrollment_date, status)
VALUES 
(12, 101, '2024-09-01', 'enrolled'),
(12, 102, '2024-09-01', 'enrolled'),
(12, 103, '2024-09-01', 'enrolled');
```

#### Part C: Liya's email was entered incorrectly. It should be 'liya.tadesse@university.edu'. Write the UPDATE.
```sql
UPDATE students
SET email = 'liya.tadesse@university.edu'
WHERE student_id = 12;
```

#### Part D: Liya decides to drop CS201. Write the DELETE for her enrollment in that course.
```sql
DELETE FROM enrollments 
WHERE student_id = 12 AND course_id = 102;
```

#### Part E: Write a query to show all of Liya's current enrollments with course titles.
```sql
SELECT e.enrollment_id, c.title, e.status
FROM enrollments e
JOIN courses c ON e.course_id = c.course_id
WHERE e.student_id = 12;
```

---

### Exercise 7.12: Data Cleanup Scenario
**Task:** The university is cleaning up old records.

#### Part A: Find all students who enrolled before 2023-01-01.
```sql
SELECT * FROM students WHERE enrollment_date < '2023-01-01';
```

#### Part B: Before deleting old students, you must delete their enrollments first. Write a query to find all enrollments for students who enrolled before 2023-01-01.
```sql
SELECT * FROM enrollments 
WHERE student_id IN (SELECT student_id FROM students WHERE enrollment_date < '2023-01-01');
```

#### Part C: Write the DELETE statements to remove these enrollments, then the students. Do this safely — verify before each delete.

```sql
-- Verify enrollments to delete
SELECT * FROM enrollments 
WHERE student_id IN (SELECT student_id FROM students WHERE enrollment_date < '2023-01-01');

-- Delete enrollments
DELETE FROM enrollments 
WHERE student_id IN (SELECT student_id FROM students WHERE enrollment_date < '2023-01-01');

-- Verify students to delete
SELECT * FROM students WHERE enrollment_date < '2023-01-01';

-- Delete students
DELETE FROM students WHERE enrollment_date < '2023-01-01';
```

#### Part D: What would happen if you tried to delete a student who still has enrollments, and your database has referential integrity enabled?
> 🚨 **ANSWER:** The database would block the operation and throw a **Foreign Key Constraint Violation Error** (e.g., *update or delete on table "students" violates foreign key constraint*). It completely prevents the deletion to protect data integrity and avoid orphan records in the database.

---

### Exercise 7.13: Bulk Operations
**Task:** The university needs to make several changes at the start of a new semester.

#### Part A: All courses in the Mathematics department (department_id = 20) need their credits increased by 1. Write the UPDATE.
```sql
UPDATE courses
SET credits = credits + 1
WHERE department_id = 20;
```

#### Part B: A new batch of 5 students needs to be added. Write a single INSERT statement.



| student_id | first_name | last_name | email | gpa | enrollment_date |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 13 | Mulugeta | Alemayehu | mulugeta@university.edu | 3.50 | 2024-09-01 |
| 14 | Nardos | Girma | nardos@university.edu | 3.90 | 2024-09-01 |
| 15 | Obsa | Tesfaye | obsa@university.edu | 3.20 | 2024-09-01 |
| 16 | Rahel | Bekele | rahel@university.edu | 3.85 | 2024-09-01 |
| 17 | Samuel | Hailu | samuel@university.edu | 3.60 | 2024-09-01 |

```sql


```sql
___________________
