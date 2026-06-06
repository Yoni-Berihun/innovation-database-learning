# 🔍 Filtering & Sorting Data in PostgreSQL

Welcome to Module 08! You've mastered CRUD operations — you can create, read, update, and delete records with confidence. But so far, you've mostly worked with entire tables or single records.

In the real world, databases contain thousands, millions, or even billions of records. You never want to look at all of them at once. You want to **find exactly what you need** and **see it in the right order**.

That's what this module is about.

By the end, you'll be able to filter data with precision, sort it any way you want, search for patterns in text, and limit your results to just the most important rows. These skills turn a database from a static storage box into a powerful search and analysis tool.

Let's learn to find exactly what we're looking for. 🔍

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

* Use the `WHERE` clause to filter data based on conditions
* Apply comparison operators (`=`, `!=`, `&gt;`, `&lt;`, `&gt;=`, `&lt;=`) to build precise filters
* Combine conditions with `AND` and `OR`
* Search for text patterns using the `LIKE` operator and wildcards
* Sort query results using `ORDER BY` in ascending and descending order
* Limit the number of returned rows using `LIMIT`
* Combine filtering, sorting, and limiting in a single query
* Apply these skills to the University Management System
* Avoid common beginner mistakes with quotes, operators, and sorting

---

## 🧠 Why This Matters

Imagine you're the registrar at a university with 20,000 students. A professor asks you: "Can you give me a list of all Computer Science students with a GPA above 3.5 who enrolled this semester, sorted by GPA from highest to lowest, and just the top 20?"

Without filtering and sorting, you'd have to scroll through 20,000 records manually. With SQL, you write one query and get the answer in under a second.

### Real-World Examples

| Company | What They Filter and Sort |
|---------|--------------------------|
| **Netflix** | Movies by genre, rating, release year, and your watch history |
| **Banking apps** | Transactions by date, amount, or merchant name |
| **University systems** | Students by major, GPA, enrollment status, or graduation year |
| **E-commerce sites** | Products by price, rating, category, or availability |
| **Social media** | Posts by date, popularity, or relevance to you |

&gt; 💡 **Every search, every filter, every "sort by price" or "top rated" you see on a website is powered by the exact SQL skills you're about to learn.**

### The Analogy: Searching for Books in a Library

&gt; Imagine a library with 100,000 books.
&gt;
&gt; **Without filtering:** You'd walk through every shelf, reading every spine, hoping to find what you want.
&gt;
&gt; **With filtering:** You ask the librarian: "Show me all science fiction books published after 2020 with more than 500 pages."
&gt;
&gt; **With sorting:** "And sort them by rating, highest first."
&gt;
&gt; **With limiting:** "Just show me the top 10."
&gt;
&gt; **That's exactly what SQL does.** The `WHERE` clause is your filter. `ORDER BY` is your sort. `LIMIT` is your "just show me the top results."

---

## 📖 Concept Explanation

### What is Filtering?

**Filtering** means selecting only the rows that match specific conditions. Instead of seeing everything in a table, you see only what you care about.

Think of it like a sieve — you pour in all the data, and only the pieces that fit your criteria come through.

---

### WHERE Clause

The `WHERE` clause comes after `FROM` and specifies which rows to include.

```sql
SELECT * FROM students
WHERE gpa &gt; 3.0;

### Line by line:
* `SELECT *` — get all columns
* `FROM students` — from the students table
* `WHERE gpa > 3.0` — only where the GPA is greater than 3.0
* `;` — end of command

**Result:** Only students with a GPA above 3.0.

### More WHERE Examples
```sql
-- Find a specific student
SELECT * FROM students
WHERE student_id = 5;

-- Find students who enrolled on a specific date
SELECT * FROM students
WHERE enrollment_date = '2024-09-01';

-- Find students with a specific last name
SELECT * FROM students
WHERE last_name = 'Kebede';
```

> ⚠️ Text values go in single quotes. Numbers do not.

---

## Comparison Operators

These operators let you build conditions in your `WHERE` clause.




| Operator | Meaning | Example | Result |
| :--- | :--- | :--- | :--- |
| `=` | Equal to | `WHERE gpa = 4.0` | GPA is exactly 4.0 |
| `!=` or `<>` | Not equal to | `WHERE status != 'graduated'` | Status is anything except graduated |
| `>` | Greater than | `WHERE gpa > 3.5` | GPA is above 3.5 |
| `<` | Less than | `WHERE gpa < 2.0` | GPA is below 2.0 |
| `>=` | Greater than or equal | `WHERE credits >= 3` | Credits is 3 or more |
| `<=` | Less than or equal | `WHERE credits <= 3` | Credits is 3 or less |

### Examples with Comparison Operators
```sql
-- Students with perfect GPA
SELECT * FROM students
WHERE gpa = 4.0;

-- Students who are not active
SELECT * FROM students
WHERE status != 'active';

-- Courses with 4 or more credits
SELECT * FROM courses
WHERE credits >= 4;

-- Students with GPA below passing threshold
SELECT * FROM students
WHERE gpa < 2.0;
```

---

## AND / OR Operators

Sometimes you need multiple conditions. `AND` requires all conditions to be true. `OR` requires at least one to be true.

### AND Examples
```sql
-- Students with GPA above 3.5 who enrolled in 2024
SELECT * FROM students
WHERE gpa > 3.5
  AND enrollment_date >= '2024-01-01';

-- Computer Science courses with 4 credits
SELECT * FROM courses
WHERE department_id = 10
  AND credits = 4;
```

### OR Examples
```sql
-- Students in Computer Science OR Mathematics
SELECT * FROM students
WHERE major_department_id = 10
   OR major_department_id = 20;

-- Courses with 3 or 4 credits
SELECT * FROM courses
WHERE credits = 3
   OR credits = 4;
```

### Combining AND and OR
Use parentheses to group conditions clearly.
```sql
-- Students with high GPA in either CS or Math
SELECT * FROM students
WHERE gpa > 3.5
  AND (major_department_id = 10 OR major_department_id = 20);
```

💡 Without parentheses, `AND` is evaluated before `OR`, which can lead to unexpected results. Always use parentheses when mixing `AND` and `OR`.

---

## LIKE Operator

The `LIKE` operator lets you search for text patterns. It's incredibly useful when you don't know the exact value.

You use two special characters called wildcards:



| Wildcard | Meaning | Example |
| :--- | :--- | :--- |
| `%` | Matches any sequence of characters (including none) | `'A%'` matches anything starting with A |
| `_` | Matches exactly one character | `'A_'` matches exactly two characters starting with A |

### LIKE Examples
```sql
-- Students whose first name starts with A
SELECT * FROM students
WHERE first_name LIKE 'A%';

-- Students whose last name ends with 'e'
SELECT * FROM students
WHERE last_name LIKE '%e';

-- Students whose email contains 'university'
SELECT * FROM students
WHERE email LIKE '%university%';

-- Courses with codes starting with CS
SELECT * FROM courses
WHERE course_code LIKE 'CS%';

-- Students with exactly 5-letter first names starting with A
SELECT * FROM students
WHERE first_name LIKE 'A____';
```

💡 `LIKE` is case-sensitive by default in PostgreSQL. Use `ILIKE` for case-insensitive matching.
```sql
-- Case-insensitive search
SELECT * FROM students
WHERE first_name ILIKE 'a%';
```

---

## ORDER BY

The `ORDER BY` clause sorts your results. By default, it sorts in ascending order (A to Z, smallest to largest).
```sql
-- Students sorted by GPA from lowest to highest
SELECT * FROM students
ORDER BY gpa;

-- Same thing, explicitly
SELECT * FROM students
ORDER BY gpa ASC;
```

### Descending Order
```sql
-- Students sorted by GPA from highest to lowest
SELECT * FROM students
ORDER BY gpa DESC;
```

### Sorting by Multiple Columns
```sql
-- Sort by department, then by last name within each department
SELECT * FROM students
ORDER BY major_department_id, last_name;

-- Sort by GPA descending, then by last name ascending
SELECT * FROM students
ORDER BY gpa DESC, last_name ASC;
```

---

## LIMIT

The `LIMIT` clause restricts how many rows are returned. It's perfect for "top 10" or "first 5" queries.
```sql
-- Top 5 students by GPA
SELECT * FROM students
ORDER BY gpa DESC
LIMIT 5;

-- First 3 courses alphabetically
SELECT * FROM courses
ORDER BY title
LIMIT 3;
```

💡 `LIMIT` is usually used with `ORDER BY`. Without `ORDER BY`, you get an arbitrary set of rows, which is rarely useful.

---

## 🌍 Real-World Examples

Let's apply everything to our University Management System.

### Example 1: Find Top Students
```sql
-- Students with GPA above 3.5, sorted highest first, top 10 only
SELECT first_name, last_name, gpa
FROM students
WHERE gpa > 3.5
ORDER BY gpa DESC
LIMIT 10;
```
* **Practical use:** Dean's list, scholarship candidates, honor society invitations.

### Example 2: Search for Students by Name
```sql
-- Find all students whose last name starts with T
SELECT first_name, last_name, email
FROM students
WHERE last_name LIKE 'T%'
ORDER BY last_name;
```
* **Practical use:** Registrar looking up students, directory search, mailing list generation.

### Example 3: Find Heavy Courses
```sql
-- Courses with 4 or more credits in Computer Science
SELECT course_code, title, credits
FROM courses
WHERE credits >= 4
  AND department_id = 10
ORDER BY credits DESC, title;
```
* **Practical use:** Academic advising, workload planning, prerequisite checking.

### Example 4: Recent Enrollments
```sql
-- Students who enrolled in the last 30 days, sorted by date
SELECT first_name, last_name, enrollment_date
FROM students
WHERE enrollment_date >= CURRENT_DATE - INTERVAL '30 days'
ORDER BY enrollment_date DESC;
```
* **Practical use:** New student orientation planning, welcome campaigns, reporting.

### Example 5: Find Instructors in Science Departments
```sql
-- Instructors in Computer Science or Mathematics
SELECT first_name, last_name, department_id
FROM instructors
WHERE department_id = 10 OR department_id = 20
ORDER BY department_id, last_name;
```
* **Practical use:** Department directory, committee assignments, resource allocation.

---

## 💻 Hands-On Practice

### Practice 1: Basic Filtering
```sql
-- Find all active students
SELECT * FROM students WHERE status = 'active';

-- Find courses with exactly 3 credits
SELECT * FROM courses WHERE credits = 3;

-- Find instructors hired before 2020
SELECT * FROM instructors WHERE hire_date < '2020-01-01';
```

### Practice 2: Comparison Operators
```sql
-- Students with GPA between 2.0 and 3.0
SELECT * FROM students WHERE gpa >= 2.0 AND gpa <= 3.0;

-- Courses that are NOT in department 10
SELECT * FROM courses WHERE department_id != 10;
```

### Practice 3: Pattern Matching
```sql
-- Courses with 'Introduction' in the title
SELECT * FROM courses WHERE title LIKE '%Introduction%';

-- Students with emails at university.edu
SELECT * FROM students WHERE email LIKE '%@university.edu';
```

### Practice 4: Sorting
```sql
-- Students by last name A-Z
SELECT * FROM students ORDER BY last_name;

-- Courses by credits (most first)
SELECT * FROM courses ORDER BY credits DESC;

-- Instructors by hire date (newest first)
SELECT * FROM instructors ORDER BY hire_date DESC;
```

### Practice 5: Combining Everything
```sql
-- Top 5 Computer Science students by GPA
SELECT first_name, last_name, gpa
FROM students
WHERE major_department_id = 10
ORDER BY gpa DESC
LIMIT 5;
```

---

## ⚠️ Common Beginner Mistakes

### Mistake 1: Forgetting Quotes Around Text
```sql
-- WRONG
SELECT * FROM students WHERE last_name = Kebede;

-- CORRECT
SELECT * FROM students WHERE last_name = 'Kebede';
```
* **Error:** *Column "kebede" does not exist.*

### Mistake 2: Using Wrong Operator
```sql
-- WRONG (this checks for equality, not assignment)
SELECT * FROM students WHERE gpa => 3.5;

-- CORRECT
SELECT * FROM students WHERE gpa >= 3.5;
```
* **Error:** *Syntax error. The => operator is not standard comparison.*

### Mistake 3: Confusing ASC and DESC
```sql
-- This gives lowest GPA first (probably not what you want for "top students")
SELECT * FROM students ORDER BY gpa ASC LIMIT 5;

-- This gives highest GPA first
SELECT * FROM students ORDER BY gpa DESC LIMIT 5;
```
* **Fix:** Remember — `ASC` = Ascending (smallest to largest). `DESC` = Descending (largest to smallest).

### Mistake 4: Using LIMIT Without ORDER BY
```sql
