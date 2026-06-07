# 🔒 Constraints in PostgreSQL

 **Prerequisite:** Modules 01–12  
 

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. Explain what constraints are and why they protect your data.
2. Use `NOT NULL` to prevent empty values.
3. Use `UNIQUE` to prevent duplicate values.
4. Define `PRIMARY KEY` as the unique identifier for every row.
5. Create `FOREIGN KEY` relationships to link tables safely.
6. Use `CHECK` to validate custom business rules.
7. Use `DEFAULT` to set automatic fallback values.
8. Understand how constraints prevent real-world data disasters.

---

## 🧠 Why Constraints Matter

Imagine a university where anyone can enter anything into the database:

- A student with no name.
- Two students with the same ID number.
- An enrollment for a course that does not exist.
- A grade of `150` out of `100`.
- A negative salary for an instructor.

Without constraints, bad data enters your system silently. Bad data leads to broken applications, incorrect reports, and angry users.

**Constraints are rules you attach to tables.** PostgreSQL enforces them automatically. If someone tries to break the rule, PostgreSQL says **NO** and stops the operation.

&gt; 💡 **Internship Tip:** Companies lose millions of dollars every year due to bad data. Constraints are your first line of defense. Interviewers expect you to know them.

---

## 📖 What Are Constraints?

A **constraint** is a rule applied to a column or a table that restricts the data allowed inside it.

Think of constraints as **guardrails on a highway**. They don't stop you from driving, but they prevent you from driving off a cliff.

### Without Constraints

```sql
INSERT INTO students (student_id, first_name, gpa)
VALUES (NULL, NULL, -5.0);
-- PostgreSQL accepts this. Your data is now garbage.
```

### With Constraints
```sql
INSERT INTO students (student_id, first_name, gpa)
VALUES (NULL, NULL, -5.0);
-- PostgreSQL rejects this. Your data stays clean.
```

---

## 📖 NOT NULL

### Concept
`NOT NULL` ensures a column **cannot contain empty (NULL) values**.

### Analogy
A university ID card must have a student name printed on it. You cannot issue a blank ID card.

### Example
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

#### What Happens?
```sql
INSERT INTO students (first_name, last_name, email)
VALUES ('Alice', 'Johnson', 'alice@uni.edu');
-- ✅ Accepted. All required fields have values.

INSERT INTO students (first_name, last_name, email)
VALUES ('Bob', NULL, 'bob@uni.edu');
-- ❌ Rejected! last_name cannot be NULL.
```

#### Real-World Use Case
* Every student must have a name and email.
* Every course must have a title.
* Every grade must have a numeric value.

---

## 📖 UNIQUE

### Concept
`UNIQUE` ensures no two rows can have the same value in that column.

### Analogy
Every student must have a unique email address. Two students cannot share the same university email.

### Example
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE
);
```

#### What Happens?
```sql
INSERT INTO students (first_name, last_name, email)
VALUES ('Alice', 'Johnson', 'alice@uni.edu');
-- ✅ Accepted.

INSERT INTO students (first_name, last_name, email)
VALUES ('Bob', 'Smith', 'alice@uni.edu');
-- ❌ Rejected! Email already exists.
```

#### Real-World Use Case
* Student emails must be unique.
* Course codes must be unique.
* Department names must be unique.

💡 **Note:** `PRIMARY KEY` automatically includes `NOT NULL` and `UNIQUE`.

---

## 📖 PRIMARY KEY

### Concept
A `PRIMARY KEY` is a column (or group of columns) that uniquely identifies every row in a table.

### Analogy
A student ID number. No two students share the same ID, and every student must have one.

### Example
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE
);
```

#### What Happens?
```sql
INSERT INTO students (first_name, last_name, email)
VALUES ('Alice', 'Johnson', 'alice@uni.edu');
-- ✅ Accepted. student_id auto-generates as 1.

INSERT INTO students (student_id, first_name, last_name, email)
VALUES (1, 'Bob', 'Smith', 'bob@uni.edu');
-- ❌ Rejected! student_id 1 already exists.
```

#### Real-World Use Case
* `student_id` identifies students.
* `course_id` identifies courses.
* `instructor_id` identifies instructors.

💡 **Rule:** Every table should have a primary key. It is the foundation of relational databases.

---

## 📖 FOREIGN KEY

### Concept
A `FOREIGN KEY` links two tables together. It ensures that a value in one table must exist in another table.

### Analogy
You cannot enroll in a course that does not exist. The enrollment record points to a real course.

### Diagram
```text
┌─────────────────┐         ┌─────────────────┐
│   students      │         │   enrollments   │
├─────────────────┤         ├─────────────────┤
│ student_id (PK) │◄────────│ student_id (FK) │
│ first_name      │         │ course_id (FK)  │
│ last_name       │         │ semester        │
└─────────────────┘         └─────────────────┘
                                    │
                                    ▼
                            ┌─────────────────┐
                            │    courses      │
                            ├─────────────────┤
                            │ course_id (PK)  │
                            │ title           │
                            │ credits         │
                            └─────────────────┘
```
* **PK** = Primary Key *(unique identifier)*
* **FK** = Foreign Key *(reference to another table's PK)*
* *The arrow shows the relationship direction.*

### Example
```sql
CREATE TABLE enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    semester VARCHAR(20) NOT NULL,
    
    CONSTRAINT fk_student
        FOREIGN KEY (student_id) REFERENCES students(student_id),
    
    CONSTRAINT fk_course
        FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

#### What Happens?
```sql
INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 101, 'Fall 2025');
-- ✅ Accepted if student 1 and course 101 exist.

INSERT INTO enrollments (student_id, course_id, semester)
VALUES (999, 101, 'Fall 2025');
-- ❌ Rejected! Student 999 does not exist.
```

### ON DELETE and ON UPDATE
You can define what happens when the referenced row changes:




| Action | Meaning |
| :--- | :--- |
| **NO ACTION (default)** | Reject the deletion if foreign keys still reference it. |
| **CASCADE** | Automatically delete/update referencing rows. |
| **SET NULL** | Set the foreign key to NULL (only if column allows NULL). |
| **SET DEFAULT** | Set the foreign key to its default value. |

```sql
CONSTRAINT fk_student
    FOREIGN KEY (student_id) REFERENCES students(student_id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
```

#### Real-World Use Case
* Prevent enrollments for deleted courses.
* Prevent grades for non-existent students.
* Ensure every instructor belongs to a real department.

---

## 📖 CHECK

### Concept
`CHECK` defines a custom condition that every value in a column must satisfy.

### Analogy
A university rule says GPA must be between 0.0 and 4.0. A `CHECK` constraint enforces this rule automatically.

### Example
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    gpa NUMERIC(3,2) CHECK (gpa >= 0.0 AND gpa <= 4.0)
);
```

#### What Happens?
```sql
INSERT INTO students (first_name, last_name, email, gpa)
VALUES ('Alice', 'Johnson', 'alice@uni.edu', 3.50);
-- ✅ Accepted.

INSERT INTO students (first_name, last_name, email, gpa)
VALUES ('Bob', 'Smith', 'bob@uni.edu', 5.00);
-- ❌ Rejected! GPA must be between 0.0 and 4.0.
```

### Named CHECK Constraint
```sql
CREATE TABLE grades (
    grade_id SERIAL PRIMARY KEY,
    enrollment_id INT NOT NULL,
    letter_grade CHAR(2) NOT NULL,
    numeric_grade INT CHECK (numeric_grade >= 0 AND numeric_grade <= 100),
    
    CONSTRAINT valid_letter_grade
        CHECK (letter_grade IN ('A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D+', 'D', 'F'))
);
```

#### Real-World Use Case
* GPA must be between 0.0 and 4.0.
* Course credits must be between 1 and 6.
* Salaries must be positive numbers.
* Enrollment dates must be in the future.

---

## 📖 DEFAULT

### Concept
`DEFAULT` sets an automatic value if no value is provided during insertion.

### Analogy
If a student does not specify a preferred contact method, the system defaults to "Email."

### Example
```sql
CREATE TABLE enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    semester VARCHAR(20) NOT NULL,
    enrollment_date DATE DEFAULT CURRENT_DATE,
    
    CONSTRAINT fk_student
        FOREIGN KEY (student_id) REFERENCES students(student_id),
    CONSTRAINT fk_course
        FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

#### What Happens?
```sql
INSERT INTO enrollments (student_id, course_id, semester)
VALUES (1, 101, 'Fall 2025');
-- ✅ Accepted. enrollment_date automatically set to today's date.

INSERT INTO enrollments (student_id, course_id, semester, enrollment_date)
VALUES (2, 102, 'Fall 2025', '2025-08-15');
-- ✅ Accepted. enrollment_date is explicitly set.
```

#### Real-World Use Case
* Enrollment date defaults to today.
* Student status defaults to "Active."
* Grade defaults to "In Progress."
* Created-at timestamp defaults to current time.

---

## 🌍 Real-World Examples

### Example 1: Complete Students Table
```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    gpa NUMERIC(3,2) CHECK (gpa >= 0.0 AND gpa <= 4.0),
    enrollment_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'Active'
        CHECK (status IN ('Active', 'Inactive', 'Graduated', 'Suspended'))
);
```

### Example 2: Complete Courses Table
```sql
CREATE TABLE courses (
```

