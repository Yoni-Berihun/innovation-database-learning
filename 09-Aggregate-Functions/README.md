# 📊 Aggregate Functions in PostgreSQL

Welcome to Module 09! You've learned to filter data, sort it, and join multiple tables together. Now it's time to learn one of SQL's most powerful features: **aggregate functions**.

Think about this: your university has 20,000 students. The dean asks, "What is the average GPA of all Computer Science students?" Without aggregate functions, you'd have to export all the data, open a calculator, and manually compute the average. With aggregate functions, you write one line of SQL and get the answer in milliseconds.

Aggregate functions are how databases **summarize** information. They take thousands of rows and boil them down to a single meaningful number. Every report, every dashboard, every "summary statistic" you see online is powered by these functions.

By the end of this module, you'll be able to count records, calculate averages, find minimums and maximums, and sum values across entire tables or filtered groups. These are the tools that turn raw data into actionable insights.

Let's start summarizing. 📊

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

* Explain what aggregate functions are and why they matter
* Use `COUNT()` to count rows in a table or filtered result
* Use `SUM()` to add numeric values together
* Use `AVG()` to calculate the average of numeric values
* Use `MIN()` to find the smallest value in a column
* Use `MAX()` to find the largest value in a column
* Combine aggregate functions with `WHERE` for filtered summaries
* Apply aggregate functions to the University Management System
* Understand when aggregate functions can and cannot be used

---

## 🧠 Why Aggregate Functions Matter

Every organization in the world needs to summarize data. Not the details — the big picture.

### Real-World Examples

| Organization | Question They Ask | Aggregate Function |
|-------------|-------------------|-------------------|
| **University** | How many students enrolled this year? | COUNT() |
| **University** | What is the average GPA of graduating seniors? | AVG() |
| **Bank** | What is the total amount deposited today? | SUM() |
| **Netflix** | What is the highest-rated movie this month? | MAX() |
| **Hospital** | What is the lowest blood pressure recorded today? | MIN() |
| **E-commerce** | How many orders were placed last week? | COUNT() |
| **E-commerce** | What is the average order value? | AVG() |

&gt; 💡 **Aggregate functions are the tools of decision-making.** When a manager asks "How are we doing?" the answer comes from aggregates. When a professor asks "How did my class perform?" the answer comes from aggregates. When you ask "What's my bank balance?" — well, that's just one row, but your monthly spending summary is aggregates.

### The Analogy: The Teacher's Gradebook

&gt; Imagine a teacher with a gradebook containing 30 students and 10 assignments each. That's 300 individual grades.
&gt;
&gt; The teacher doesn't read all 300 grades to parents. Instead, they summarize:
&gt;
&gt; * "The class average is 82%" — that's AVG()
&gt; * "The highest score was 98%" — that's MAX()
&gt; * "The lowest score was 65%" — that's MIN()
&gt; * "Total points possible: 1000" — that's SUM()
&gt; * "30 students took the test" — that's COUNT()
&gt;
&gt; **Aggregate functions do exactly this — but for millions of rows in milliseconds.**

---

## 📖 What Are Aggregate Functions?

**Aggregate functions** are SQL functions that take multiple rows as input and return a single summary value.

Instead of looking at every row individually, you ask the database to compute something across all of them.

| Function | What It Computes | Input | Output |
|----------|-----------------|-------|--------|
| `COUNT()` | Number of rows | Any column or `*` | A number |
| `SUM()` | Total of values | Numeric column | A number |
| `AVG()` | Average of values | Numeric column | A number |
| `MIN()` | Smallest value | Any comparable column | One value |
| `MAX()` | Largest value | Any comparable column | One value |

&gt; 💡 **Key idea:** Aggregate functions collapse many rows into one result. If you have 10,000 students, `COUNT(*)` returns one number: 10000.

---

## 📖 COUNT()

`COUNT()` returns the number of rows that match a condition.

### Count All Rows

```sql
SELECT COUNT(*) FROM students;
### Count with a Condition
```sql
SELECT COUNT(*) FROM students
WHERE gpa > 3.5;
```
**Result:** The number of students with a GPA above 3.5.

### Count Specific Column (Ignores NULLs)
```sql
SELECT COUNT(gpa) FROM students;
```
**Result:** The number of students who have a GPA value (not `NULL`).

💡 `COUNT(*)` counts all rows. `COUNT(column)` counts only rows where that column is not `NULL`.

### Practical Example: University Enrollment
```sql
-- How many students are enrolled this semester?
SELECT COUNT(*) FROM enrollments WHERE status = 'enrolled';

-- How many courses does the university offer?
SELECT COUNT(*) FROM courses;

-- How many instructors are in the Computer Science department?
SELECT COUNT(*) FROM instructors WHERE department_id = 10;
```

---

## 📖 SUM()

`SUM()` adds up all the values in a numeric column.

### Total Credits Offered
```sql
SELECT SUM(credits) FROM courses;
```

#### Line by line:
* `SELECT SUM(credits)` — add all values in the credits column
* `FROM courses` — from the courses table
* `;` — end of command

**Result:** The total number of credit hours offered by the university.

### Total Department Budget
```sql
SELECT SUM(budget) FROM departments;
```
**Result:** The combined budget of all departments.

### Sum with a Condition
```sql
-- Total credits in Computer Science courses
SELECT SUM(credits) FROM courses WHERE department_id = 10;
```
**Result:** The total credits for all CS courses.

> ⚠️ `SUM()` only works with numeric columns. You cannot sum text or dates.

---

## 📖 AVG()

`AVG()` calculates the average of all values in a numeric column.

### Average Student GPA
```sql
SELECT AVG(gpa) FROM students;
```

#### Line by line:
* `SELECT AVG(gpa)` — calculate the average of the gpa column
* `FROM students` — from the students table
* `;` — end of command

**Result:** The average GPA across all students.

### Average with a Condition
```sql
-- Average GPA of active students
SELECT AVG(gpa) FROM students WHERE status = 'active';

-- Average course credits
SELECT AVG(credits) FROM courses;
```

💡 `AVG()` automatically ignores `NULL` values. If some students have no GPA recorded, they don't bring the average down.

---

## 📖 MIN()

`MIN()` finds the smallest value in a column.

### Lowest GPA
```sql
SELECT MIN(gpa) FROM students;
```
**Result:** The lowest GPA among all students.

### Earliest Hire Date
```sql
SELECT MIN(hire_date) FROM instructors;
```
**Result:** The date when the most senior instructor was hired.

### Minimum with a Condition
```sql
-- Lowest GPA among active students
SELECT MIN(gpa) FROM students WHERE status = 'active';
```

---

## 📖 MAX()

`MAX()` finds the largest value in a column.

### Highest GPA
```sql
SELECT MAX(gpa) FROM students;
```
**Result:** The highest GPA among all students.

### Latest Enrollment Date
```sql
SELECT MAX(enrollment_date) FROM students;
```
**Result:** The most recent date a student enrolled.

### Maximum with a Condition
```sql
-- Highest budget among all departments
SELECT MAX(budget) FROM departments;

-- Most credits for a single course
SELECT MAX(credits) FROM courses;
```

---

## 🌍 Real-World Example: University Statistics

Let's see how a university registrar uses aggregate functions to generate reports.

### Report 1: Student Body Overview
```sql
-- Total number of students
SELECT COUNT(*) AS total_students FROM students;

-- Average GPA
SELECT AVG(gpa) AS average_gpa FROM students;

-- Highest and lowest GPA
SELECT MAX(gpa) AS highest_gpa, MIN(gpa) AS lowest_gpa FROM students;
```

#### Result:



| total_students | average_gpa | highest_gpa | lowest_gpa |
| :--- | :--- | :--- | :--- |
| 20000 | 3.24 | 4.00 | 0.00 |

### Report 2: Department Performance
```sql
-- Total budget across all departments
SELECT SUM(budget) AS total_budget FROM departments;

-- Average department budget
SELECT AVG(budget) AS average_budget FROM departments;

-- Department with highest budget
SELECT MAX(budget) AS highest_budget FROM departments;
```

### Report 3: Course Catalog Summary
```sql
-- Total courses offered
SELECT COUNT(*) AS total_courses FROM courses;

-- Total credits offered
SELECT SUM(credits) AS total_credits FROM courses;

-- Average credits per course
SELECT AVG(credits) AS average_credits FROM courses;
```

### Report 4: Enrollment Statistics
```sql
-- Total enrollments this semester
SELECT COUNT(*) AS total_enrollments FROM enrollments;

-- Enrollments by status
SELECT COUNT(*) AS enrolled_count FROM enrollments WHERE status = 'enrolled';
SELECT COUNT(*) AS completed_count FROM enrollments WHERE status = 'completed';
SELECT COUNT(*) AS dropped_count FROM enrollments WHERE status = 'dropped';
```

---

## 💻 Hands-On Practice

### Practice 1: COUNT
```sql
-- Count all students
SELECT COUNT(*) FROM students;

-- Count students with GPA above 3.0
SELECT COUNT(*) FROM students WHERE gpa > 3.0;

-- Count courses in the CS department
SELECT COUNT(*) FROM courses WHERE department_id = 10;
```

### Practice 2: SUM
```sql
-- Total credits in the catalog
SELECT SUM(credits) FROM courses;

-- Total budget for Science departments (10 and 20)
SELECT SUM(budget) FROM departments WHERE department_id IN (10, 20);
```

### Practice 3: AVG
```sql
-- Average student GPA
SELECT AVG(gpa) FROM students;

-- Average course credits
SELECT AVG(credits) FROM courses;
```

### Practice 4: MIN and MAX
```sql
-- GPA range
SELECT MIN(gpa) AS lowest, MAX(gpa) AS highest FROM students;

-- Budget range
SELECT MIN(budget) AS smallest_budget, MAX(budget) AS largest_budget FROM departments;
```

### Practice 5: Combined Report
```sql
-- Complete university summary in one query
SELECT
    COUNT(*) AS total_students,
    AVG(gpa) AS average_gpa,
    MIN(gpa) AS lowest_gpa,
    MAX(gpa) AS highest_gpa
FROM students;
```

---

## ⚠️ Common Beginner Mistakes

### Mistake 1: Using SUM() on Text
```sql
-- WRONG
SELECT SUM(first_name) FROM students;

-- CORRECT
SELECT COUNT(first_name) FROM students;
```
* **Error:** *Cannot sum text values. Use COUNT for text, SUM for numbers.*

### Mistake 2: Forgetting Parentheses
```sql
-- WRONG
SELECT COUNT * FROM students;

-- CORRECT
SELECT COUNT(*) FROM students;
```
* **Error:** *Missing parentheses. All aggregate functions require them.*

### Mistake 3: Expecting Individual Rows
```sql
-- This returns ONE row, not one per student
SELECT COUNT(*), gpa FROM students;
```
* **Error:** *Mixing aggregates with regular columns without GROUP BY (covered in next module).*

### Mistake 4: Using AVG() on Integers Unexpectedly
```sql
-- If credits are integers, AVG may return integer-like results
SELECT AVG(credits) FROM courses;
```
* **Note:** PostgreSQL returns exact numeric averages, but some databases truncate. Be aware of data types.

### Mistake 5: NULL Confusion
```sql
-- COUNT(*) counts all rows including NULLs
SELECT COUNT(*) FROM students;

-- COUNT(column) ignores NULLs in that column
SELECT COUNT(gpa) FROM students;
```
* **Note:** If some students have no GPA, these two queries return different results.

---

## ✅ Quick Check Questions

1. What does `COUNT(*)` do? How is it different from `COUNT(column)`?
2. Write a query to find the total budget of all departments.
3. What does `AVG(gpa)` compute? What happens if some GPA values are `NULL`?
4. Write a query to find the highest and lowest GPA in one result.
5. Which aggregate function would you use to find how many courses have 4 credits?

---

## 🎯 Key Takeaways

* 📊 **Aggregate functions** summarize many rows into a single result.
* 🔢 **COUNT()** counts rows. `COUNT(*)` counts all rows. `COUNT(column)` ignores `NULL`s.
* ➕ **SUM()** adds numeric values together. Only works with numbers.
* 📈 **AVG()** calculates the average. Automatically ignores `NULL` values.
* ⬇️ **MIN()** finds the smallest value in a column.
* ⬆️ **MAX()** finds the largest value in a column.
* 🔍 Combine aggregates with `WHERE` to summarize filtered data.
* 🏫 The **University Management System** uses aggregates for enrollment reports, GPA statistics, and budget summaries.
* ⚠️ **Common mistakes** include using `SUM` on text, forgetting parentheses, and confusing `COUNT(*)` with `COUNT(column)`.

---

## 📚 Learning Resources

### 🎥 YouTube Videos



| Video | Why It's Useful | Link |
| :--- | :--- | :--- |
| **SQL Aggregate Functions (freeCodeCamp)** | Comprehensive explanation of COUNT, SUM, AVG, MIN, MAX with practical examples. Perfect for beginners. | [Watch on YouTube](https://youtube.com) |
