
---

# Exercises.md


# Module 13: Exercises — Constraints in PostgreSQL

> **Instructions:** Complete each exercise by writing the appropriate SQL. Use the University Management System schema. Test your queries in a local PostgreSQL instance or on [pgExercises](https://pgexercises.com) / [SQLZoo](https://sqlzoo.net).

---

## 🟢 Easy Exercises

### Exercise 1: NOT NULL Constraint
**Task:** Create a `departments` table with the following requirements:
- `department_id`: primary key, auto-increment
- `name`: cannot be empty, must be unique
- `building`: cannot be empty

Write the `CREATE TABLE` statement.

---

### Exercise 2: UNIQUE Constraint
**Task:** Create an `instructors` table with the following requirements:
- `instructor_id`: primary key, auto-increment
- `first_name` and `last_name`: cannot be empty
- `email`: cannot be empty, must be unique

Write the `CREATE TABLE` statement.

---

### Exercise 3: PRIMARY KEY
**Task:** Explain in one sentence why the `student_id` column in the `students` table should be a `PRIMARY KEY` instead of just `UNIQUE`.

---

### Exercise 4: FOREIGN KEY Basics
**Task:** Create an `enrollments` table that links to `students` and `courses`. Requirements:
- `enrollment_id`: primary key
- `student_id`: cannot be empty, must reference `students(student_id)`
- `course_id`: cannot be empty, must reference `courses(course_id)`
- `semester`: cannot be empty

Write the `CREATE TABLE` statement.

---

### Exercise 5: CHECK Constraint
**Task:** Create a `courses` table with the following requirements:
- `course_id`: primary key
- `title`: cannot be empty
- `credits`: must be between 1 and 6

Write the `CREATE TABLE` statement.

---

## 🟡 Medium Exercises

### Exercise 6: DEFAULT Values
**Task:** Modify the `students` table (or recreate it) so that:
- `enrollment_date` defaults to today's date
- `status` defaults to `'Active'`

Write the `CREATE TABLE` statement.

---

### Exercise 7: Named Constraints
**Task:** Create a `grades` table with:
- `grade_id`: primary key
- `enrollment_id`: references `enrollments(enrollment_id)`
- `letter_grade`: must be one of `A, A-, B+, B, B-, C+, C, C-, D+, D, F`
- `numeric_grade`: must be between 0 and 100

Name the constraint for letter grades `valid_letter_grade`.

Write the `CREATE TABLE` statement.

---

### Exercise 8: ON DELETE Behavior
**Task:** Create a `courses` table where:
- `course_id`: primary key
- `title`: cannot be empty
- `instructor_id`: references `instructors(instructor_id)`
- If an instructor is deleted, their courses should keep the instructor as NULL instead of being deleted

Write the `CREATE TABLE` statement with the appropriate `ON DELETE` behavior.

---

### Exercise 9: Combined Constraints
**Task:** Create a complete `students` table with ALL of the following constraints:
- `student_id`: primary key
- `first_name`, `last_name`, `email`: NOT NULL
- `email`: UNIQUE
- `gpa`: between 0.0 and 4.0
- `enrollment_date`: defaults to today
- `status`: defaults to `'Active'`, must be one of `'Active', 'Inactive', 'Graduated', 'Suspended'`

Write the `CREATE TABLE` statement.

---

### Exercise 10: Testing Constraints
**Task:** For the `students` table you created in Exercise 9, write three `INSERT` statements:
1. One that should succeed
2. One that should fail because of a `CHECK` constraint
3. One that should fail because of a `UNIQUE` constraint

---

## 🔴 Challenging Exercises

### Exercise 11: Multi-Column UNIQUE
**Task:** In the `enrollments` table, ensure that a student cannot enroll in the same course twice in the same semester. This requires a `UNIQUE` constraint across multiple columns.

Write the `CREATE TABLE` statement with this multi-column unique constraint.

---

### Exercise 12: Self-Referencing Foreign Key
**Task:** Create a `courses` table where some courses can have prerequisites. Add a column `prerequisite_course_id` that references `course_id` in the same table. Ensure that if a prerequisite course is deleted, the prerequisite link is set to NULL.

Write the `CREATE TABLE` statement.

---

### Exercise 13: Complete Schema Design
**Task:** Design and write `CREATE TABLE` statements for ALL six University Management System tables (`students`, `instructors`, `departments`, `courses`, `enrollments`, `grades`) with appropriate constraints for every table. Include:
- Primary keys on every table
- Foreign keys where relationships exist
- NOT NULL on all required fields
- CHECK constraints where appropriate
- DEFAULT values where appropriate

Write all six `CREATE TABLE` statements.

---

### Exercise 14: Alter Table Constraints
**Task:** Imagine the `students` table already exists without constraints. Write `ALTER TABLE` statements to:
1. Add a `CHECK` constraint that GPA must be between 0.0 and 4.0
2. Add a `DEFAULT` value of `'Active'` to the `status` column
3. Add a `FOREIGN KEY` from `students.department_id` to `departments.department_id`

Write the three `ALTER TABLE` statements.

---

### Exercise 15: Constraint Troubleshooting
**Task:** You are given the following buggy table definition. Identify at least 5 problems and rewrite it correctly.

```sql
CREATE TABLE enrollments (
    enrollment_id INT,
    student_id INT,
    course_id INT,
    semester VARCHAR,
    grade VARCHAR
);
