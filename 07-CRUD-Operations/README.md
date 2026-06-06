# 🔄 CRUD Operations in PostgreSQL

Welcome to Module 07! In the previous module, you learned your first SQL commands — SELECT, INSERT, UPDATE, and DELETE. You wrote queries that talked to your database and got results back. That was exciting, but we only scratched the surface.

Now we're going to dive deep into the **four operations that power every database application in the world**. Together, they form the acronym **CRUD** — Create, Read, Update, Delete. Every app you use, from Instagram to your banking app, runs thousands of CRUD operations every second.

Think about it: when you post a photo on Instagram, that's a **Create**. When you scroll your feed, that's a **Read**. When you edit your profile, that's an **Update**. When you delete a comment, that's a **Delete**.

By the end of this module, you'll be able to perform all four operations confidently and safely in PostgreSQL. You'll understand why the `WHERE` clause is your best friend for updates and deletes. And you'll see how CRUD connects directly to the University Management System we've been building together.

Let's master CRUD. 🚀

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

* Explain what CRUD stands for and why it's the foundation of all database applications
* Write `INSERT` statements to create new records with single and multiple rows
* Write `SELECT` statements to read data with specific columns and conditions
* Write `UPDATE` statements to modify existing records safely
* Write `DELETE` statements to remove records without destroying your data
* Use the `WHERE` clause precisely to target exactly the right records
* Understand the difference between `DELETE` and `DROP` (and why confusing them is dangerous)
* Apply CRUD operations to the University Management System
* Avoid common beginner mistakes that can corrupt or lose data

---

## 🧠 Why CRUD Matters

Every database application in the world — every single one — is built on CRUD operations.

### Real-World Examples

| Application | Create | Read | Update | Delete |
|-------------|--------|------|--------|--------|
| **Instagram** | Post a photo | Scroll your feed | Edit your caption | Delete a comment |
| **Student Registration** | Enroll in a course | View your schedule | Change your major | Drop a course |
| **Bank Account** | Open a new account | Check your balance | Update your address | Close an account |
| **E-commerce** | Add a product to catalog | Browse products | Change the price | Remove sold-out items |
| **University System** | Add a new student | View student records | Update a student's GPA | Remove a withdrawn student |

&gt; 💡 **CRUD is the heartbeat of data.** Without these four operations, databases would be static, lifeless containers. CRUD makes data dynamic, useful, and alive.

### The Analogy: CRUD Is Like Managing a Notebook

&gt; Imagine you keep a notebook with information about your university courses.
&gt;
&gt; **Create:** You write a new entry for a course you just enrolled in.
&gt;
&gt; **Read:** You flip through the notebook to find your schedule.
&gt;
&gt; **Update:** You cross out an old professor's name and write the new one.
&gt;
&gt; **Delete:** You tear out the page for a course you dropped.
&gt;
&gt; **A database does the same thing — but with millions of entries, instantly, without mistakes, and with multiple people using it at the same time.**

---

## 📖 What is CRUD?

**CRUD** is an acronym that describes the four basic operations you can perform on data in any database.

| Letter | Operation | SQL Command | What It Does |
|--------|-----------|-------------|--------------|
| **C** | Create | `INSERT` | Adds new data to a table |
| **R** | Read | `SELECT` | Retrieves data from a table |
| **U** | Update | `UPDATE` | Modifies existing data |
| **D** | Delete | `DELETE` | Removes data from a table |

These four operations are so fundamental that every other database concept builds on them. Joins, aggregations, subqueries, views — they all start with CRUD.

&gt; 🎯 **Master CRUD, and you've mastered the foundation of everything else.**

---

## ➕ CREATE (INSERT)

The **Create** operation adds new records to a table. In SQL, we use the `INSERT` statement.

### Inserting One Record

```sql
INSERT INTO students
(student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES
(1, 'Abel', 'Kebede', 'abel@university.edu', 3.85, '2024-09-01');

### Line by line:
* `INSERT INTO students` — add data to the students table
* `(student_id, first_name, last_name, email, gpa, enrollment_date)` — the columns we're filling
* `VALUES` — here come the actual values
* `(1, 'Abel', 'Kebede', 'abel@university.edu', 3.85, '2024-09-01')` — the data, in the same order as the columns
* `;` — end of command

> ⚠️ Text goes in single quotes. Numbers don't. Dates go in single quotes.

### Inserting Multiple Records
```sql
INSERT INTO students
(student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES
(2, 'Birtukan', 'Tadesse', 'birtukan@university.edu', 3.60, '2024-09-01'),
(3, 'Chala', 'Bekele', 'chala@university.edu', 3.20, '2023-09-01'),
(4, 'Desta', 'Hailu', 'desta@university.edu', 3.90, '2024-09-01');
```
**What happens:** Three new students are added in a single command.

### Inserting Without Specifying All Columns
If you want to insert only some columns, you can — but the others must allow `NULL` or have default values.
```sql
INSERT INTO students
(student_id, first_name, last_name, email)
VALUES
(5, 'Eyerus', 'Alemu', 'eyerus@university.edu');
```
**What happens:** `gpa` and `enrollment_date` will be `NULL` (if allowed) or use their default values.

### Inserting into the Courses Table
```sql
INSERT INTO courses
(course_id, course_code, title, credits, department_id)
VALUES
(101, 'CS101', 'Introduction to Programming', 4, 10),
(102, 'CS201', 'Database Systems', 3, 10),
(103, 'MATH101', 'Calculus I', 4, 20);
```

---

## 📖 READ (SELECT)

The Read operation retrieves data from a table. In SQL, we use the `SELECT` statement.

### Select All Columns
```sql
SELECT * FROM students;
```
**Result:** Every column and every row from the `students` table.

### Select Specific Columns
```sql
SELECT first_name, last_name, email
FROM students;
```
**Result:** Only the three columns you asked for, for all students.

### Select with a Condition (WHERE)
```sql
SELECT first_name, last_name, gpa
FROM students
WHERE gpa > 3.5;
```
**Result:** Only students whose GPA is greater than 3.5.

### Select with Multiple Conditions
```sql
SELECT first_name, last_name, email
FROM students
WHERE gpa > 3.5
  AND enrollment_date = '2024-09-01';
```
**Result:** Students with GPA above 3.5 who enrolled on September 1, 2024.

### Select and Sort Results
```sql
SELECT first_name, last_name, gpa
FROM students
ORDER BY gpa DESC;
```
**Result:** All students sorted by GPA from highest to lowest.

💡 `DESC` means descending (highest to lowest). `ASC` means ascending (lowest to highest).

---

## ✏️ UPDATE

The Update operation modifies existing records. In SQL, we use the `UPDATE` statement.

### Update One Record
```sql
UPDATE students
SET gpa = 3.95
WHERE student_id = 1;
```

#### Line by line:
* `UPDATE students` — modify the students table
* `SET gpa = 3.95` — change the GPA to 3.95
* `WHERE student_id = 1` — only for Abel (student_id = 1)
* `;` — end of command

> ⚠️ **Always use WHERE with UPDATE. Without it, every record changes.**

### Update Multiple Columns
```sql
UPDATE students
SET email = 'abel.kebede@university.edu',
    gpa = 3.95
WHERE student_id = 1;
```
**What happens:** Abel's email AND GPA are updated in one command.

### Update Based on a Condition
```sql
UPDATE courses
SET credits = 4
WHERE department_id = 10;
```
**What happens:** All courses in the Computer Science department (`department_id = 10`) now have 4 credits.

🚨 Before running `UPDATE`, run a `SELECT` with the same `WHERE` clause to see what you'll change.
```sql
-- First, check what you're about to update
SELECT * FROM courses WHERE department_id = 10;

-- Then, update
UPDATE courses
SET credits = 4
WHERE department_id = 10;
```

---

## ❌ DELETE

The Delete operation removes records from a table. In SQL, we use the `DELETE` statement.

### Delete One Record
```sql
DELETE FROM students
WHERE student_id = 5;
```

#### Line by line:
* `DELETE FROM students` — remove from the students table
* `WHERE student_id = 5` — only Eyerus (student_id = 5)
* `;` — end of command

> ⚠️ **Always use WHERE with DELETE. Without it, you delete everything.**

### What Happens Without WHERE
```sql
DELETE FROM students;
```
**Result:** Every student is deleted. The table is empty. There is no undo.

🚨 This is one of the most dangerous commands in SQL. Always double-check your `WHERE` clause.

### Safe Deletion Practice
```sql
-- Step 1: Verify what you'll delete
SELECT * FROM students WHERE student_id = 5;

-- Step 2: Delete
DELETE FROM students WHERE student_id = 5;

-- Step 3: Confirm it's gone
SELECT * FROM students WHERE student_id = 5;
```

### DELETE vs DROP



| Command | What It Does | Danger Level |
| :--- | :--- | :--- |
| `DELETE FROM students WHERE ...` | Removes specific rows | Medium (if you use WHERE) |
| `DELETE FROM students;` | Removes ALL rows | High |
| `DROP TABLE students;` | Deletes the entire table forever | Extreme |

💡 `DELETE` removes data. `DROP` removes structure. Be very careful with `DROP`.

---

## 🌍 Real-World Example: University Registration System

Let's walk through a complete registration scenario using all four CRUD operations.

### Scenario: New Semester at the University

#### Step 1: CREATE — Add new students
```sql
INSERT INTO students
(student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES
(6, 'Fikir', 'Dawit', 'fikir@university.edu', 3.40, '2024-09-01'),
(7, 'Genet', 'Solomon', 'genet@university.edu', 3.95, '2024-09-01');
```

#### Step 2: READ — View all current students
```sql
SELECT student_id, first_name, last_name, gpa
FROM students
WHERE enrollment_date = '2024-09-01'
ORDER BY gpa DESC;
```

#### Step 3: CREATE — Enroll students in courses
```sql
INSERT INTO enrollments
(enrollment_id, student_id, course_id, enrollment_date, status)
VALUES
(1001, 6, 101, '2024-09-01', 'enrolled'),
(1002, 6, 102, '2024-09-01', 'enrolled'),
(1003, 7, 101, '2024-09-01', 'enrolled');
```

#### Step 4: READ — Check who's in Introduction to Programming
```sql
SELECT s.first_name, s.last_name
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
WHERE e.course_id = 101;
```

#### Step 5: UPDATE — Fix a typo in an email
```sql
UPDATE students
SET email = 'fikir.dawit@university.edu'
WHERE student_id = 6;
```

#### Step 6: READ — Verify the update
```sql
SELECT student_id, first_name, email
FROM students
WHERE student_id = 6;
```

#### Step 7: DELETE — A student withdraws
```sql
-- First, remove enrollment records
DELETE FROM enrollments
WHERE student_id = 7;

-- Then, remove the student
DELETE FROM students
WHERE student_id = 7;
```

#### Step 8: READ — Confirm the withdrawal
```sql
SELECT * FROM students WHERE student_id = 7;
-- Should return no rows

SELECT * FROM enrollments WHERE student_id = 7;
-- Should also return no rows
```

💡 **Notice the order:** We deleted enrollments first, then the student. If we deleted the student first, the enrollments would become "orphan" records — unless our database has referential integrity rules that handle this automatically.

---

## ⚠️ Common Beginner Mistakes

### Mistake 1: Forgetting the Semicolon
```sql
SELECT * FROM students
```
* **What happens:** PostgreSQL waits for more input, thinking you're still typing.
* **Fix:** Add `;` at the end of every SQL statement.

### Mistake 2: Forgetting WHERE on UPDATE
```sql
UPDATE students SET gpa = 4.0;
```
* **What happens:** Every student's GPA becomes 4.0.
* **Fix:** Always include `WHERE`: `UPDATE students SET gpa = 4.0 WHERE student_id = 1;`

### Mistake 3: Forgetting WHERE on DELETE
```sql
DELETE FROM students;
```
* **What happens:** All students are gone forever.
* **Fix:** Always include `WHERE`: `DELETE FROM students WHERE student_id = 1;`

### Mistake 4: Wrong Data Types
```sql
INSERT INTO students (gpa) VALUES ('excellent');
```
* **What happens:** Error — GPA expects a number, not text.
* **Fix:** Use the correct type: `VALUES (3.85);`

### Mistake 5: Missing Quotes on Text
```sql
INSERT INTO students (first_name) VALUES (Abel);
```
* **What happens:** Error — PostgreSQL thinks Abel is a column name.
* **Fix:** Use single quotes: `VALUES ('Abel');`

### Mistake 6: Typos in Table or Column Names
```sql
SELECT * FROM student;
```
* **What happens:** Error — table is named `students`, not `student`.
* **Fix:** Check your spelling. PostgreSQL table names are case-sensitive when quoted.

### Mistake 7: Confusing DELETE and DROP
```sql
DROP TABLE students;
```
* **What happens:** The entire table structure is deleted — not just the data.
### 🧪 Practice Platforms




| Platform | Why It's Useful | Link |
| :--- | :--- | :--- |
| **SQLBolt** | Interactive lessons covering INSERT, SELECT, UPDATE, DELETE with immediate feedback. Perfect for practicing CRUD. | [Start Learning](https://sqlbolt.com) |
| **SQLZoo** | Hands-on SQL practice with real datasets. Great for applying CRUD to different scenarios. | [Start Practicing](https://sqlzoo.net) |
| **HackerRank SQL** | Coding challenges that test your CRUD skills. Good for building confidence. | [Start Practicing](https://hackerrank.com) |
| **DataLemur** | SQL interview questions from real companies. Preview of job-ready CRUD skills. | [Start Practicing](https://datalemur.com) |

---

🚀 **You're ready for the exercises!** CRUD is the heartbeat of database work. Every query you'll ever write builds on these four operations. Practice them until they feel natural — because you'll use them every single day as a database developer.

💬 **Remember:** The best database professionals are paranoid about `UPDATE` and `DELETE`. They always check their `WHERE` clause twice. They always verify with `SELECT` first. This caution isn't fear — it's professionalism. Adopt it now, and you'll save yourself countless hours of data recovery later. 🎯
