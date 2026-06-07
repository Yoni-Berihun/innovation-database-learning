 SQL Joins in PostgreSQL

&gt; **Prerequisite:** Module 9 (Filtering Data with `WHERE`)  

---

## Table of Contents
1. [What Are Joins?](#what-are-joins)
2. [Why Joins Matter](#why-joins-matter)
3. [The University Management System Schema](#the-university-management-system-schema)
4. [INNER JOIN](#inner-join)
5. [LEFT JOIN](#left-join)
6. [RIGHT JOIN](#right-join)
7. [FULL JOIN](#full-join)
8. [Join Diagrams](#join-diagrams)
9. [Learning Resources](#learning-resources)
10. [Key Takeaways](#key-takeaways)

---

## What Are Joins?

In a relational database like PostgreSQL, data is stored across multiple tables to keep things organized and avoid repetition. For example, student names live in the `students` table, and course details live in the `courses` table.

But in the real world, you rarely need data from just *one* table. You might need to know: *"Which students are enrolled in which courses?"* or *"Which instructors teach in which departments?"*

A **JOIN** is a SQL operation that combines rows from two or more tables based on a related column between them. It is the bridge that connects your data.

### Beginner-Friendly Analogy
Imagine you have two Excel sheets:
- **Sheet 1:** A list of students with their `student_id`.
- **Sheet 2:** A list of enrollments with `student_id` and `course_id`.

If you want to see the student's *name* next to the course they are taking, you need to look up the name from Sheet 1 using the `student_id` from Sheet 2. A JOIN does exactly this lookup automatically.

---

## Why Joins Matter

| Without Joins | With Joins |
|---------------|------------|
| You run one query for students, then another for enrollments, and manually match them in your head or in code. | You write one query. PostgreSQL does the matching for you instantly. |
| Slow, error-prone, and hard to maintain. | Fast, accurate, and scalable. |

**Real-world relevance:**
- A university registrar needs a report of all students and their grades. That data spans `students`, `enrollments`, and `grades`.
- A course scheduling app needs to show the instructor's name alongside the course. That spans `courses` and `instructors`.
- A payroll system needs to link employees to departments. That spans two tables.

If you want to build real applications, **you must master joins**. They are the most frequently used SQL feature after `SELECT` and `WHERE`.

---

## The University Management System Schema

We will use these tables for all examples in this module.

### 1. `students`
| student_id | first_name | last_name | email | department_id |
|------------|------------|-----------|-------|---------------|
| 1 | Alice | Johnson | alice@uni.edu | 1 |
| 2 | Bob | Smith | bob@uni.edu | 2 |
| 3 | Charlie | Brown | charlie@uni.edu | NULL |
| 4 | Diana | Prince | diana@uni.edu | 1 |

### 2. `courses`
| course_id | title | credits | instructor_id |
|-----------|-------|---------|---------------|
| 101 | Intro to CS | 3 | 10 |
| 102 | Database Systems | 4 | 11 |
| 103 | Web Development | 3 | NULL |

### 3. `instructors`
| instructor_id | first_name | last_name | department_id |
|---------------|------------|-----------|---------------|
| 10 | Dr. Alan | Turing | 1 |
| 11 | Dr. Grace | Hopper | 2 |
| 12 | Dr. Tim | Berners-Lee | NULL |

### 4. `departments`
| department_id | name |
|---------------|------|
| 1 | Computer Science |
| 2 | Mathematics |

### 5. `enrollments`
| enrollment_id | student_id | course_id | semester |
|---------------|------------|-----------|----------|
| 500 | 1 | 101 | Fall 2025 |
| 501 | 1 | 102 | Fall 2025 |
| 502 | 2 | 101 | Fall 2025 |
| 503 | 3 | 103 | Fall 2025 |

### 6. `grades`
| grade_id | enrollment_id | letter_grade | numeric_grade |
|----------|---------------|--------------|---------------|
| 900 | 500 | A | 95 |
| 901 | 501 | B | 85 |
| 902 | 502 | A | 92 |

---

## INNER JOIN

### Concept
`INNER JOIN` returns only the rows that have **matching values in both tables**. If there is no match, the row is discarded.

### Syntax
```sql
SELECT columns
FROM table_a
INNER JOIN table_b
  ON table_a.common_column = table_b.common_column;

### Example: List Students and Their Enrolled Courses
We want to see the student's name and the course title they are taking.

```sql
SELECT
  s.first_name,
  s.last_name,
  c.title
FROM students s
INNER JOIN enrollments e
  ON s.student_id = e.student_id
INNER JOIN courses c
  ON e.course_id = c.course_id;
```

#### Result



| first_name | last_name | title |
| :--- | :--- | :--- |
| Alice | Johnson | Intro to CS |
| Alice | Johnson | Database Systems |
| Bob | Smith | Intro to CS |
| Charlie | Brown | Web Development |

> 📌 **Notice:** Diana Prince is not in the result because she has no enrollments. The `INNER JOIN` only keeps records that have matching data in both tables.

#### Real-World Use Case
A professor wants a class roster. They need names from students linked to enrollments. An `INNER JOIN` gives exactly the students who signed up — eliminating empty or unlinked rows.

---

## LEFT JOIN

### Concept
`LEFT JOIN` (or `LEFT OUTER JOIN`) returns all rows from the left table, and the matched rows from the right table. If there is no match, the result is `NULL` on the right side.

### Syntax
```sql
SELECT columns
FROM table_a
LEFT JOIN table_b
  ON table_a.common_column = table_b.common_column;
```

### Example: List All Students and Their Enrollments (Including Non-Enrolled)
We want to see every student, even if they haven't enrolled in anything yet.

```sql
SELECT
  s.first_name,
  s.last_name,
  c.title
FROM students s
LEFT JOIN enrollments e
  ON s.student_id = e.student_id
LEFT JOIN courses c
  ON e.course_id = c.course_id;
```

#### Result



| first_name | last_name | title |
| :--- | :--- | :--- |
| Alice | Johnson | Intro to CS |
| Alice | Johnson | Database Systems |
| Bob | Smith | Intro to CS |
| Charlie | Brown | Web Development |
| Diana | Prince | *NULL* |

> 📌 **Notice:** Diana Prince appears with `NULL` for the course title because she has no enrollments. `LEFT JOIN` ensures she is not excluded from the list.

#### Real-World Use Case
The university wants to identify students who have not yet registered for any courses. A `LEFT JOIN` combined with a `WHERE c.course_id IS NULL` filter isolates them instantly.

---

## RIGHT JOIN

### Concept
`RIGHT JOIN` (or `RIGHT OUTER JOIN`) returns all rows from the right table, and the matched rows from the left table. If there is no match, the result is `NULL` on the left side. It is essentially the mirror image of `LEFT JOIN`.

### Syntax
```sql
SELECT columns
FROM table_a
RIGHT JOIN table_b
  ON table_a.common_column = table_b.common_column;
```

### Example: List All Courses and Their Instructors (Including Unassigned Courses)
We want to see every course, even if no instructor is assigned yet.

```sql
SELECT
  c.title,
  i.first_name,
  i.last_name
FROM instructors i
RIGHT JOIN courses c
  ON i.instructor_id = c.instructor_id;
```

#### Result



| title | first_name | last_name |
| :--- | :--- | :--- |
| Intro to CS | Dr. Alan | Turing |
| Database Systems | Dr. Grace | Hopper |
| Web Development | *NULL* | *NULL* |

> 📌 **Notice:** Web Development has no instructor assigned (`NULL`), but it still appears because the `courses` table is on the right side of the join.

#### Practical Note
`RIGHT JOIN` is used less frequently than `LEFT JOIN` in real-world applications. Most developers prefer to restructure the query to use `LEFT JOIN` by swapping the table order, as it is more intuitive to read from left to right. However, you must know it exists for code reviews and legacy systems.

---

## FULL JOIN

### Concept
`FULL JOIN` (or `FULL OUTER JOIN`) returns all rows when there is a match in either the left or right table. If there is no match, the result is `NULL` on the side that lacks the match.

### Syntax
```sql
SELECT columns
FROM table_a
FULL JOIN table_b
  ON table_a.common_column = table_b.common_column;
```

### Example: List All Instructors and All Departments (Even Unmatched)
We want to see every instructor and every department, regardless of whether they are linked.

```sql
SELECT
  i.first_name,
  i.last_name,
  d.name AS department_name
FROM instructors i
FULL JOIN departments d
  ON i.department_id = d.department_id;
```

#### Result



| first_name | last_name | department_name |
| :--- | :--- | :--- |
| Dr. Alan | Turing | Computer Science |
| Dr. Grace | Hopper | Mathematics |
| Dr. Tim | Berners-Lee | *NULL* |

> 💡 **Why is a department listed once?** The row with `NULL` instructors would only appear if there was a department with no instructors. In our sample data, both departments have instructors, so we see Dr. Berners-Lee (no department) alongside all matched pairs.

#### Real-World Use Case
A data audit to find orphaned records: *"Show me all instructors without departments AND all departments without instructors."* A `FULL JOIN` gives you both datasets in a single query loop.

---

## Join Diagrams

Visualizing joins helps beginners understand what data is kept and what is discarded.

### Venn Diagrams
```text
INNER JOIN:     Only the overlapping center.
LEFT JOIN:      The entire left circle + overlap.
RIGHT JOIN:     The entire right circle + overlap.
FULL JOIN:      Both entire circles.
```

### Table Representation

#### 1. INNER JOIN
*Students INNER JOIN Enrollments*



| student_id | name | enrollment_id | course_id |
| :--- | :--- | :--- | :--- |
| 1 | Alice | 500 | 101 |
| 1 | Alice | 501 | 102 |
| 2 | Bob | 502 | 101 |
| 3 | Charlie | 503 | 103 |

*(Diana (4) is excluded because she has no active enrollment record.)*

#### 2. LEFT JOIN
*Students LEFT JOIN Enrollments*



| student_id | name | enrollment_id | course_id |
| :--- | :--- | :--- | :--- |
| 1 | Alice | 500 | 101 |
| 1 | Alice | 501 | 102 |
| 2 | Bob | 502 | 101 |
| 3 | Charlie | 503 | 103 |
| 4 | Diana | *NULL* | *NULL* |

*(Diana is explicitly included in the result set using NULL placeholders.)*

#### 3. RIGHT JOIN
*Instructors RIGHT JOIN Courses*



| instructor_id | name | course_id | title |
| :--- | :--- | :--- | :--- |
| 10 | Dr. Turing | 101 | Intro to CS |
| 11 | Dr. Hopper | 102 | Database Systems |
| *NULL* | *NULL* | 103 | Web Development |

*(Web Development is included in the output rows even without an assigned instructor.)*

#### 4. FULL JOIN
*Instructors FULL JOIN Departments*



| instructor_id | instructor_name | department_id | department_name |
| :--- | :--- | :--- | :--- |
| 10 | Dr. Turing | 1 | Computer Science |
| 11 | Dr. Hopper | 2 | Mathematics |
| 12 | Dr. Berners-Lee | *NULL* | *NULL* |

---

## 📚 Learning Resources

### 🎥 YouTube Videos



| Video | Why It's Useful | Link |
| :--- | :--- | :--- |
| **"SQL Joins Explained" by Fireship** | Visual, fast-paced 10-minute conceptual high-level explanation. | [Watch on YouTube](https://youtube.com) |
| **"SQL Joins Tutorial for Beginners" by Mosh** | Professional 20-minute conceptual deep dive into table connections. | [Watch on YouTube](https://youtube.com) |
| **"PostgreSQL Tutorial - JOINS" by freeCodeCamp** | Hands-on code executions inside a comprehensive learning track. | [Watch on YouTube](https://youtube.com) |

### 📄 Official Documentation



| Resource | Why It's Useful | Link |
| :--- | :--- | :--- |
| **PostgreSQL 16: Table Expressions - Joined Tables** | Authoritative internal layout mappings and query parse tree limits. | [Read Documentation](https://postgresql.org) |
| **PostgreSQL 16: SELECT Syntax** | Developer grammar baseline blueprint constraints. | [Read Documentation](https://postgresql.org) |

### 🌍 Articles
* **"A Visual Explanation of SQL Joins" by Jeff Atwood (Coding Horror)** — Classic software development blog post displaying concepts using Venn diagrams.
* **"SQL JOIN Types Explained" by DataCamp** — Interactive text examples focusing on query design choices.
* **"PostgreSQL JOINs" by PostgreSQL Tutorial** — Practical blueprints mirroring corporate system administration workflows.

### 🧪 Practice Platforms
* **pgExercises (pgexercises.com)** — Free, sandbox environment targeting PostgreSQL operational workflows.
* **SQLBolt (sqlbolt.com)** — In-browser editor compiling syntax outputs row-by-row.
* **HackerRank (hackerrank.com)** — Competitive syntax challenges testing data schema structures.
* **LeetCode (leetcode.com)** — Technical interview challenge mock database environments.

---

## 🎯 Key Takeaways

* 🔗 **JOINs connect tables:** They allow you to query and reference datasets spread across distinct structures as if they resided within a single file.
* 🚪 **INNER JOIN is the default configuration:** It strictly preserves intersecting record loops that hold identical target keys across both tables.
* ⬅️ **LEFT JOIN keeps the base data:** It ensures every single row within the left (primary) table is returned, inserting a blank `NULL` reference wherever right-side parameters are absent.
* ➡️ **RIGHT JOIN reverses the alignment:** It acts as the mirror configuration of a left join, pulling unlinked entries from the right side.
* 🌐 **FULL JOIN retains the entire dataset:** It prevents any entries from being dropped, making it ideal for tracking orphaned database data points.
* 🔑 **Map key fields deliberately:** Always clearly identify the shared column relationship (typically mapping a Primary Key directly to a Foreign Key).
* 🏷️ **Deploy table aliases consistently:** Shorten strings (e.g., `students s`) to optimize code legibility and block ambiguity runtime compilation blocks.
