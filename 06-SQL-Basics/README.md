# 🧠 SQL Basics — Talking to Your Database

Welcome to Module 06! You've installed PostgreSQL, created your first database, and run a few simple queries. Now it's time to learn the language that makes databases come alive: **SQL**.

Think of everything you've learned so far as learning the rules of a board game. You know what the pieces are (tables, rows, columns), how they connect (keys, relationships), and what the board looks like (ER diagrams). But you haven't learned how to *play* yet.

SQL is how you play. It's how you ask questions, add information, change records, and remove data. It's the language that turns a static database into a living, responsive system.

By the end of this module, you'll be able to create, read, update, and delete data in your University Management System. These four operations — known as **CRUD** — are the foundation of every database application in the world.

Let's start talking to your database. 🗣️

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

* Explain what SQL is and why it's the standard language for databases
* Write a `SELECT` statement to retrieve data from a table
* Write an `INSERT` statement to add new records
* Write an `UPDATE` statement to modify existing records
* Write a `DELETE` statement to remove records safely
* Understand why the `WHERE` clause is critical for updates and deletions
* Avoid common beginner SQL mistakes
* Feel confident running your first real database operations

---

## 🧠 Why Learn SQL?

Every time you check your bank balance, search for a product on Amazon, or register for a university course, SQL is running behind the scenes.

SQL is the **lingua franca** of data. It's the one language that works across almost every relational database system — PostgreSQL, MySQL, SQL Server, Oracle, and more. Learn SQL once, and you can use it everywhere.

### Where SQL Is Used

| Industry | How SQL Is Used |
|----------|----------------|
| **Banking** | Checking balances, processing transfers, detecting fraud |
| **Universities** | Managing enrollments, recording grades, generating transcripts |
| **Netflix** | Recommending shows, tracking watch history, managing subscriptions |
| **E-commerce** | Processing orders, tracking inventory, analyzing customer behavior |
| **Healthcare** | Managing patient records, scheduling appointments, billing |
| **Social Media** | Storing posts, tracking likes, finding friends |

&gt; 💡 **SQL is everywhere.** Even if you become a web developer, data analyst, product manager, or business analyst, SQL will be one of your most valuable skills. It's consistently ranked among the top 3 most in-demand technical skills in job postings.

### The Analogy: SQL Is Like Giving Instructions

&gt; Imagine you're at a restaurant.
&gt;
&gt; You don't walk into the kitchen and cook your own food. You tell the waiter what you want: "I'd like the grilled salmon with a side salad, please."
&gt;
&gt; The waiter translates your request into instructions for the kitchen. The kitchen prepares exactly what you asked for and brings it to your table.
&gt;
&gt; **SQL is your waiter.** You write instructions in a language the database understands. The database does the work and returns exactly what you asked for.
&gt;
&gt; `SELECT first_name FROM students WHERE student_id = 1;`
&gt;
&gt; Translation: "Please bring me the first name of the student whose ID is 1."

---

## 📖 What is SQL?

**SQL** stands for **Structured Query Language**.

It's a standardized language designed specifically for managing data in relational databases. You use SQL to:

| Task | SQL Statement | What It Does |
|------|-------------|--------------|
| Retrieve data | `SELECT` | Ask the database for information |
| Add data | `INSERT` | Put new records into a table |
| Modify data | `UPDATE` | Change existing records |
| Remove data | `DELETE` | Remove records from a table |
| Create structure | `CREATE` | Make new tables and databases |
| Remove structure | `DROP` | Delete tables and databases |

&gt; 🎯 **SQL is not a programming language like Python or Java.** You can't build a website with SQL. But you can ask incredibly complex questions about data, and get answers in milliseconds. That's its superpower.

### SQL Is Almost English

One of the best things about SQL is that it reads almost like English.

| SQL | English Translation |
|-----|---------------------|
| `SELECT * FROM students;` | "Select everything from the students table." |
| `SELECT name FROM courses WHERE credits &gt; 3;` | "Select the name from courses where credits are greater than 3." |
| `INSERT INTO students VALUES (...);` | "Insert into the students table these values." |

This readability makes SQL one of the most approachable technical skills for beginners.

---

## 🏗️ Understanding Tables

Before we write SQL, let's look at the tables we'll be working with. These are the tables from our University Management System.

### The Students Table

| student_id | first_name | last_name | email | gpa | enrollment_date |
|------------|-----------|-----------|-------|-----|-----------------|
| 1 | Abel | Kebede | abel@email.com | 3.85 | 2024-09-01 |
| 2 | Birtukan | Tadesse | birtukan@email.com | 3.60 | 2024-09-01 |
| 3 | Chala | Bekele | chala@email.com | 3.20 | 2023-09-01 |
| 4 | Desta | Hailu | desta@email.com | 3.90 | 2024-09-01 |

**What we see:**
* **Rows** (horizontal): Each row is one student — one complete record
* **Columns** (vertical): Each column is one attribute — first name, email, GPA
* **Records:** Another word for rows. Four students = four records

### The Courses Table

| course_id | course_code | title | credits | department_id |
|-----------|-------------|-------|---------|---------------|
| 101 | CS101 | Introduction to Programming | 4 | 10 |
| 102 | CS201 | Database Systems | 3 | 10 |
| 103 | MATH101 | Calculus I | 4 | 20 |
| 104 | ENG101 | English Composition | 3 | 30 |

### The Departments Table

| department_id | name | building | budget |
|---------------|------|----------|--------|
| 10 | Computer Science | Tech Building | 500000 |
| 20 | Mathematics | Science Hall | 300000 |
| 30 | English | Arts Building | 250000 |

&gt; 💡 **These tables are connected.** The `department_id` in the `courses` table references the `departments` table. The `student_id` in future enrollment records will reference the `students` table. This is the relational model in action.

---

## 🚀 Your First SQL Query

Let's run some simple queries to build confidence.

### Query 1: Hello SQL

sql
SELECT 'Hello SQL!';

### Line by line:
* `SELECT` — tells the database you want to retrieve something
* `'Hello SQL!'` — the text you want to display
* `;` — ends the command (like a period at the end of a sentence)

### Result:


| ?column? |
| :--- |
| Hello SQL! |

🎉 You just ran your first meaningful SQL query! The database received your instruction, processed it, and returned exactly what you asked for.

### Query 2: Simple Math
```sql
SELECT 5 + 10;
```

### Result:


| ?column? |
| :--- |
| 15 |

PostgreSQL can do calculations. This seems simple, but it's powerful — databases can compute averages, sums, and complex formulas across millions of rows.

### Query 3: Current Date
```sql
SELECT current_date;
```

### Result: 
`Today's date`

This shows that PostgreSQL has built-in functions that know things about the world. We'll explore more of these in later modules.

---

## 📥 SELECT Statement

The `SELECT` statement is the most common SQL command. It's how you ask the database for information.

### Select Everything
```sql
SELECT * FROM students;
```

#### Line by line:
* `SELECT` — retrieve data
* `*` — everything (all columns)
* `FROM` — from which table
* `students` — the table name
* `;` — end of command

**Result:** All columns and all rows from the `students` table

### Select Specific Columns
```sql
SELECT first_name, email
FROM students;
```

#### Line by line:
* `SELECT first_name, email` — only these two columns
* `FROM students` — from the students table

#### Result:


| first_name | email |
| :--- | :--- |
| Abel | abel@email.com |
| Birtukan | birtukan@email.com |
| Chala | chala@email.com |
| Desta | desta@email.com |

> 💡 **Best practice:** Only select the columns you need. `SELECT *` is convenient for learning, but in real applications, requesting unnecessary columns slows things down.

### Select with a Condition
```sql
SELECT first_name, last_name
FROM students
WHERE gpa > 3.5;
```

#### Line by line:
* `SELECT first_name, last_name` — get these two columns
* `FROM students` — from the students table
* `WHERE gpa > 3.5` — only where the GPA is greater than 3.5

#### Result:


| first_name | last_name |
| :--- | :--- |
| Abel | Kebede |
| Desta | Hailu |

🎯 **The WHERE clause is your filter.** It lets you ask precise questions. Without `WHERE`, you get everything. With `WHERE`, you get exactly what matches your condition.

---

## ➕ INSERT Statement

`INSERT` adds new records to a table. This is how data gets into your database.

### Inserting One Record
```sql
INSERT INTO students
(student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES
(5, 'Eyerus', 'Alemu', 'eyerus@email.com', 3.75, '2024-09-01');
```

#### Line by line:
* `INSERT INTO students` — add data to the students table
* `(student_id, first_name, last_name, email, gpa, enrollment_date)` — the columns you're providing values for
* `VALUES` — here come the actual values
* `(5, 'Eyerus', 'Alemu', 'eyerus@email.com', 3.75, '2024-09-01')` — the actual data
* `;` — end of command

**What happens:** A new row appears in the `students` table with Eyerus's information.

> ⚠️ **Important:** Text values must be in single quotes `'like this'`. Numbers don't need quotes. Dates go in single quotes too.

### Inserting Multiple Records
```sql
INSERT INTO students
(student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES
(6, 'Fikir', 'Dawit', 'fikir@email.com', 3.40, '2024-09-01'),
(7, 'Genet', 'Solomon', 'genet@email.com', 3.95, '2024-09-01'),
(8, 'Hana', 'Mekonnen', 'hana@email.com', 3.10, '2024-09-01');
```

**What happens:** Three new rows are added at once.

💡 The column list is optional if you provide values for **ALL** columns in the correct order. But including the column list is safer — it prevents errors if the table structure changes.

---

## ✏️ UPDATE Statement

`UPDATE` changes existing records. This is how you fix typos, update information, or modify records as circumstances change.

### Updating One Record
```sql
UPDATE students
SET email = 'newabel@email.com'
WHERE student_id = 1;
```

#### Line by line:
* `UPDATE students` — modify the students table
* `SET email = 'newabel@email.com'` — change the email column to this new value
* `WHERE student_id = 1` — only for the student whose ID is 1
* `;` — end of command

**What happens:** Abel's email changes from `abel@email.com` to `newabel@email.com`.

> ⚠️ **CRITICAL:** Always use `WHERE` with `UPDATE`. Without `WHERE`, you change **EVERY** record in the table.

### What Happens Without WHERE
```sql
UPDATE students
SET email = 'newabel@email.com';
```

**Result:** Every single student now has the email `newabel@email.com`. This is almost never what you want.

🚨 This is one of the most dangerous mistakes in SQL. Always double-check your `WHERE` clause before running `UPDATE` or `DELETE`.

### Updating Multiple Columns
```sql
UPDATE students
SET email = 'abel.kebede@university.edu',
    gpa = 3.90
WHERE student_id = 1;
```

**What happens:** Abel's email AND GPA are updated in one command.

---

## ❌ DELETE Statement

`DELETE` removes records from a table. Use it carefully.

### Deleting One Record
```sql
DELETE FROM students
WHERE student_id = 1;
```

#### Line by line:
* `DELETE FROM students` — remove from the students table
* `WHERE student_id = 1` — only the student whose ID is 1
* `;` — end of command

**What happens:** Abel's record is removed from the table.

> ⚠️ **CRITICAL:** Always use `WHERE` with `DELETE`. Without `WHERE`, you delete **EVERY** record.

### What Happens Without WHERE
```sql
DELETE FROM students;
```

**Result:** Every single student is deleted. The table becomes empty.

🚨 There is no undo button in SQL. Once you delete data, it's gone. Always use `WHERE` and always double-check before running `DELETE`.

---

## 🌍 Real-World Example: Managing the University

Let's see how these four commands work together in our University Management System.

### Scenario: New Semester Registration

#### Step 1: Add new students
```sql
INSERT INTO students
(student_id, first_name, last_name, email, gpa, enrollment_date)
VALUES
(9, 'Israel', 'Tesfaye', 'israel@email.com', 3.50, '2024-09-01'),
(10, 'Jember', 'Worku', 'jember@email.com', 3.80, '2024-09-01');
```

#### Step 2: View all students to confirm
```sql
SELECT * FROM students;
```

#### Step 3: Fix a typo in an email
```sql
UPDATE students
SET email = 'israel.tesfaye@university.edu'
WHERE student_id = 9;
```

#### Step 4: A student drops out — remove their record
```sql
DELETE FROM students
WHERE student_id = 10;
```

#### Step 5: View remaining students
```sql
SELECT first_name, last_name, email
FROM students
WHERE enrollment_date = '2024-09-01';
```

💡 This is the **CRUD** cycle in action: **C**reate, **R**ead, **U**pdate, **D**elete. Every database application in the world does these four operations.

---

## ⚠️ Common Beginner Mistakes

### Mistake 1: Forgetting the Semicolon
```sql
SELECT * FROM students
```
* **Problem:** No semicolon at the end.
* **What happens:** PostgreSQL waits for more input, thinking you're still typing.
* **Fix:** Add `;` at the end of every SQL statement.

### Mistake 2: Missing Quotes Around Text
```sql
INSERT INTO students (first_name) VALUES (Abel);
```
* **Problem:** Text values need single quotes.
* **What happens:** PostgreSQL thinks `Abel` is a column name, not text.
* **Fix:** Use single quotes: `VALUES ('Abel');`

### Mistake 3: Forgetting WHERE on UPDATE
```sql
UPDATE students SET gpa = 4.0;
```
* **Problem:** No `WHERE` clause.
* **What happens:** Every student's GPA becomes 4.0.
* **Fix:** Always include `WHERE`: `UPDATE students SET gpa = 4.0 WHERE student_id = 1;`

### Mistake 4: Typo in Table Name
```sql
SELECT * FROM student;
```
* **Problem:** Table is named `students` (plural), not `student`.
* **What happens:** Error: *"relation 'student' does not exist"*
* **Fix:** Check your table names carefully. PostgreSQL is case-sensitive for quoted names.

### Mistake 5: Wrong Data Type
```sql
INSERT INTO students (gpa) VALUES ('excellent');
```
* **Problem:** GPA expects a number, not text.
* **What happens:** Error about type mismatch.
* **Fix:** Use the correct data type: `VALUES (3.85);`

---

## 💻 Hands-On Practice

### Practice 1: SELECT Exercises
Run these queries in pgAdmin and observe the results:
```sql
-- Get all columns from the courses table
SELECT * FROM courses;

-- Get only course titles
SELECT title FROM courses;

-- Get courses with more than 3 credits
SELECT title, credits FROM courses WHERE credits > 3;
```

### Practice 2: INSERT Exercises
Add these records to your database:
```sql
-- Add a new department
INSERT INTO departments (department_id, name, building, budget)
VALUES (40, 'Physics', 'Science Hall', 350000);

-- Add a new course
INSERT INTO courses (course_id, course_code, title, credits, department_id)

### Practice 3: UPDATE Exercises
```sql
-- Update a department's budget
UPDATE departments
SET budget = 550000
WHERE department_id = 10;

-- Verify the change
SELECT * FROM departments WHERE department_id = 10;
```

### Practice 4: DELETE Exercises
```sql
-- First, see what you're about to delete
SELECT * FROM courses WHERE course_id = 105;

-- Then delete it
DELETE FROM courses WHERE course_id = 105;

-- Verify it's gone
SELECT * FROM courses;
```

> 💡 **Best practice:** Before deleting, run a `SELECT` with the same `WHERE` clause to confirm you're deleting the right records.

---

## ✅ Quick Check Questions

1. What does SQL stand for, and what is its primary purpose?
2. What is the difference between `SELECT *` and `SELECT column_name`?
3. Why is the `WHERE` clause essential in `UPDATE` and `DELETE` statements?
4. What happens if you run `DELETE FROM students` without a `WHERE` clause?
5. In an `INSERT` statement, why must text values be enclosed in single quotes?

---

## 🎯 Key Takeaways

* 🗣️ **SQL (Structured Query Language)** is the standard language for talking to relational databases.
* 📥 **SELECT** retrieves data. Use `*` for all columns, or name specific columns. Use `WHERE` to filter.
* ➕ **INSERT** adds new records. List columns and provide matching values in parentheses.
* ✏️ **UPDATE** modifies existing records. Always use `WHERE` or you'll change everything.
* ❌ **DELETE** removes records. Always use `WHERE` or you'll delete everything.
* 🎓 The **University Management System** uses these four commands to manage students, courses, and departments.
* ⚠️ **Common mistakes** include forgetting semicolons, missing quotes, omitting `WHERE`, typos, and wrong data types.
* 🔄 **CRUD** (Create, Read, Update, Delete) is the foundation of every database application.
* 🛡️ **There is no undo in SQL.** Always verify with `SELECT` before running `UPDATE` or `DELETE`.

---

## 📚 Learning Resources

### 🎥 YouTube Videos



| Video | Why It's Useful | Link |
| :--- | :--- | :--- |
| **SQL Tutorial - Full Database Course for Beginners (freeCodeCamp)** | Comprehensive 4-hour course covering all SQL basics with practical examples. Perfect for beginners who want to see SQL in action. | [Watch on YouTube](https://youtube.com) |
| **SQL for Beginners (Programming with Mosh)** | Clear, professional SQL introduction with real-world examples. Mosh explains concepts simply and builds progressively. | [Watch on YouTube](https://youtube.com) |
| **Learn SQL in 1 Hour (Amigoscode)** | Fast-paced, friendly introduction to SQL fundamentals. Great for reviewing what you learned in this module. | [Watch on YouTube](https://youtube.com) |
| **SQL Full Course (Bro Code)** | Comprehensive beginner course covering SELECT, INSERT, UPDATE, DELETE, and beyond. Excellent for coding along. | [Watch on YouTube](https://youtube.com) |


 You're ready for the next module! You can now create, read, update, and delete data. These four operations are the heartbeat of every database. In upcoming modules, we'll learn to filter precisely, sort results, combine tables, and write complex queries. Your SQL journey is well underway.
💬 Remember: Every SQL expert started with SELECT 'Hello SQL!'. The commands you learned today are the same ones used by engineers managing databases with billions of rows. Master the basics, and everything advanced becomes possible. Keep going! 🎯
