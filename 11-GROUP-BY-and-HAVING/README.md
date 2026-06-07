# 📊 GROUP BY & HAVING in PostgreSQL

&gt; **Module 11** | **Skill Level:** Beginner | **Prerequisite:** Modules 01–10  
&gt; **Estimated Time:** 2–3 hours

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. Explain what `GROUP BY` does and why it is useful.
2. Use `GROUP BY` with aggregate functions like `COUNT()`, `AVG()`, `SUM()`, `MIN()`, and `MAX()`.
3. Use `HAVING` to filter grouped results.
4. Understand the difference between `WHERE` and `HAVING`.
5. Apply `GROUP BY` and `HAVING` to real-world university management scenarios.
6. Avoid common beginner mistakes when grouping data.

---

## 🧠 Why GROUP BY and HAVING Matter

In the real world, raw data is rarely useful by itself. Decision-makers need summaries, trends, and comparisons.

### University Examples
- **How many students are in each department?**
- **What is the average GPA of students in Computer Science?**
- **Which instructors teach more than 3 courses?**
- **Which departments have more than 50 students?**

### How Companies Use Grouped Data
- **Banking reports:** Total deposits per branch, average account balance per customer segment.
- **Netflix analytics:** Most-watched genres per region, average viewing time per user demographic.
- **E-commerce dashboards:** Total sales per product category, average order value per month.
- **University systems:** Enrollment trends, department performance, graduation rates.

&gt; 💡 **Internship Tip:** Interviewers love asking `GROUP BY` questions because they test whether you can turn raw data into actionable insights.

---

## 📖 What is GROUP BY?

`GROUP BY` organizes rows into groups based on one or more columns. It is like sorting students into classrooms by their department.

### Beginner-Friendly Analogy

Imagine you have a pile of 500 student ID cards on your desk. You need to know how many students are in each department. Instead of counting them one by one in the pile, you **sort them into separate stacks** — one stack for Computer Science, one for Mathematics, one for Physics, and so on. Then you count each stack.

`GROUP BY` does exactly this. It creates stacks (groups) of rows that share the same value in a column, then lets you calculate something for each stack.

---

## 📖 Basic GROUP BY Syntax

```sql
SELECT department_id, COUNT(*)
FROM students
GROUP BY department_id;

### Expected Output



| department_id | count |
| :--- | :--- |
| 1 | 45 |
| 2 | 30 |
| 3 | 25 |

*Translation: Department 1 has 45 students, Department 2 has 30 students, and Department 3 has 25 students.*

---

## 📖 GROUP BY with Aggregate Functions

Aggregate functions calculate a single value from a group of values. They only make sense when combined with `GROUP BY`.



| Function | What It Calculates | Example |
| :--- | :--- | :--- |
| **COUNT(*)** | Number of rows | `COUNT(*) FROM students` |
| **AVG(column)** | Average value | `AVG(gpa) FROM students` |
| **SUM(column)** | Total value | `SUM(credits) FROM courses` |
| **MIN(column)** | Smallest value | `MIN(gpa) FROM students` |
| **MAX(column)** | Largest value | `MAX(gpa) FROM students` |

### Example: Average GPA by Department
```sql
SELECT department_id, AVG(gpa) AS average_gpa
FROM students
GROUP BY department_id;
```

#### What happens:
1. PostgreSQL groups all students by their `department_id`.
2. For each group, it calculates the average `gpa`.
3. It returns one row per department with the average GPA.

#### Expected Output



| department_id | average_gpa |
| :--- | :--- |
| 1 | 3.45 |
| 2 | 3.20 |
| 3 | 3.10 |

---

## 📖 What is HAVING?

`HAVING` filters groups after they have been created. It is like saying: *"Show me only the stacks that have more than 10 cards."*

### Definition
`HAVING` filters grouped results based on a condition applied to the aggregate function.

### Example: Departments with More Than 10 Students
```sql
SELECT department_id, COUNT(*)
FROM students
GROUP BY department_id
HAVING COUNT(*) > 10;
```

### Line-by-Line Explanation



| Line | What It Does |
| :--- | :--- |
| `SELECT department_id, COUNT(*)` | Show department ID and student count. |
| `FROM students` | Look at the students table. |
| `GROUP BY department_id` | Group students by department. |
| `HAVING COUNT(*) > 10` | Only keep groups with more than 10 students. |

*Result: Only departments with more than 10 students appear.*

---

## 📖 WHERE vs HAVING



| Feature | WHERE | HAVING |
| :--- | :--- | :--- |
| **Filters what?** | Individual rows | Groups of rows |
| **When does it run?** | Before grouping | After grouping |
| **Can use aggregate functions?** | No | Yes |
| **Example use** | `WHERE gpa > 3.0` | `HAVING COUNT(*) > 10` |

### Visual Explanation
```text
Raw Table Rows → WHERE filters rows → GROUP BY creates groups → HAVING filters groups → Result
```

### Example: Combining WHERE and HAVING
```sql
SELECT department_id, AVG(gpa) AS average_gpa
FROM students
WHERE status = 'Active'
GROUP BY department_id
HAVING AVG(gpa) > 3.0;
```

#### What happens:
1. `WHERE status = 'Active'` — Keep only active students.
2. `GROUP BY department_id` — Group the remaining students by department.
3. `HAVING AVG(gpa) > 3.0` — Only show departments where the average GPA is above 3.0.

> ⚠️ **Rule:** Use `WHERE` to filter rows before grouping. Use `HAVING` to filter groups after grouping.

---

## 🌍 Real-World Examples

### Example 1: Departments with Average GPA Above 3.0
```sql
SELECT
    d.name AS department_name,
    AVG(s.gpa) AS average_gpa
FROM students s
JOIN departments d ON s.department_id = d.department_id
GROUP BY d.department_id, d.name
HAVING AVG(s.gpa) > 3.0;
```
* **Practical Relevance:** The dean wants to recognize high-performing departments for funding awards.

### Example 2: Instructors Teaching More Than 3 Courses
```sql
SELECT
    i.first_name || ' ' || i.last_name AS instructor_name,
    COUNT(c.course_id) AS course_count
FROM instructors i
LEFT JOIN courses c ON i.instructor_id = c.instructor_id
GROUP BY i.instructor_id, i.first_name, i.last_name
HAVING COUNT(c.course_id) > 3;
```
* **Practical Relevance:** The scheduling office needs to identify overloaded instructors and redistribute courses.

### Example 3: Departments with Many Students
```sql
SELECT
    d.name AS department_name,
    COUNT(s.student_id) AS student_count
FROM departments d
LEFT JOIN students s ON d.department_id = s.department_id
GROUP BY d.department_id, d.name
HAVING COUNT(s.student_id) > 50;
```
* **Practical Relevance:** The administration allocates lab equipment and classroom space based on department size.

### Example 4: Course Enrollment Summaries
```sql
SELECT
    c.title AS course_title,
    COUNT(e.student_id) AS enrolled_students,
    c.max_seats - COUNT(e.student_id) AS seats_remaining
FROM courses c
LEFT JOIN enrollments e ON c.course_id = e.course_id
GROUP BY c.course_id, c.title, c.max_seats;
```
* **Practical Relevance:** Students use this data during registration to find courses with open seats.

---

## 💻 Hands-On Practice

Practice the following patterns using the University Management System schema:
* Grouping rows: `GROUP BY department_id`
* Counting grouped data: `COUNT(*)`
* Averaging grouped data: `AVG(gpa)`
* Filtering grouped results: `HAVING COUNT(*) > 10`

### Quick Practice Queries
```sql
-- Practice 1: Count students per department
SELECT department_id, COUNT(*) FROM students GROUP BY department_id;

-- Practice 2: Average grade per course
SELECT c.title, AVG(g.numeric_grade)
FROM courses c
JOIN enrollments e ON c.course_id = e.course_id
JOIN grades g ON e.enrollment_id = g.enrollment_id
GROUP BY c.course_id, c.title;

-- Practice 3: Departments with more than 5 courses
SELECT department_id, COUNT(*) AS course_count
FROM courses
GROUP BY department_id
HAVING COUNT(*) > 5;
```

---

## ⚠️ Common Beginner Mistakes



| Mistake | Example | Fix |
| :--- | :--- | :--- |
| **Forgetting GROUP BY** | `SELECT department_id, COUNT(*) FROM students;` | Add `GROUP BY department_id` |
| **Using WHERE instead of HAVING** | `WHERE COUNT(*) > 10` | Use `HAVING COUNT(*) > 10` |
| **Selecting non-aggregated columns not in GROUP BY** | `SELECT name, department_id, COUNT(*)... GROUP BY department_id;` | Add `name` to `GROUP BY` or use an aggregate function. |
| **Using HAVING without GROUP BY** | `SELECT * FROM students HAVING gpa > 3.0;` | Use `WHERE` for row filtering. |
| **Grouping by the wrong column** | `GROUP BY student_id` when you want department totals | Group by `department_id` instead. |

---

## ✅ Quick Check Questions

1. What does `GROUP BY` do?
2. Why do we use `HAVING`?
3. What is the difference between `WHERE` and `HAVING`?
4. Why do we combine `GROUP BY` with aggregate functions?
5. Give one real-world use case for grouped data.

---

## 🎯 Key Takeaways

* 📊 **Organization:** `GROUP BY` organizes rows into groups based on matching column values.
* 🔢 **Calculations:** Aggregate functions (`COUNT`, `AVG`, `SUM`, `MIN`, `MAX`) calculate single values for each group.
* ⏳ **Filter Post-Group:** `HAVING` filters groups after they are created.
* 🚦 **Execution Order:** `WHERE` filters individual rows *before* grouping; `HAVING` filters groups *after* grouping.
* 🔍 **Strict Rule:** Always include non-aggregated columns that appear in your `SELECT` clause within your `GROUP BY` statement.

---

## 📚 Learning Resources

### 🎥 YouTube Videos



| Title | Why Useful | Link |
| :--- | :--- | :--- |
| **SQL GROUP BY Explained by Programming with Mosh** | Clear, slow-paced explanation with real examples. | [Watch on YouTube](https://youtube.com) |
| **SQL Aggregates & GROUP BY by freeCodeCamp** | Part of a full course, covers all aggregate functions. | [Watch on YouTube](https://youtube.com) |
| **SQL Full Course by Bro Code** | Fun, beginner-friendly, includes explicit GROUP BY section. | [Watch on YouTube](https://youtube.com) |
| **PostgreSQL Tutorial for Beginners by Amigoscode** | Practical, project-based context for all SQL concepts. | [Watch on YouTube](https://youtube.com) |

### 📄 Official Documentation
* [PostgreSQL 16: SELECT — GROUP BY Clause](https://postgresql.org) — Technical documentation on row grouping parameters.
* [PostgreSQL 16: Aggregate Functions](https://postgresql.org) — Reference guide for counting, summing, and averaging.
* [PostgreSQL 16: SELECT — HAVING Clause](https://postgresql.org) — System rules for applying post-aggregation logic conditions.

### 🌍 Articles
* **"SQL GROUP BY" by W3Schools** — Simple examples with online try-it query editors.
* **"SQL HAVING Clause" by W3Schools** — Clear, foundational side-by-side comparison with WHERE.
* **"PostgreSQL GROUP BY" by PostgreSQL Tutorial** — Practical, query-driven examples targeting university datasets.
* **"SQL GROUP BY and HAVING" by GeeksforGeeks** — Highly detailed breakdowns using execution flowchart diagrams.

### 🧪 Practice Platforms



| Platform | Why Useful | Link |
| :--- | :--- | :--- |
| **SQLBolt** | Interactive browser tutorials evaluating groupings line-by-line. | [sqlbolt.com](https://sqlbolt.com) |
| **SQLZoo** | Dedicated aggregate tracks featuring university-themed datasets. | [sqlzoo.net](https://sqlzoo.net) |
| **HackerRank SQL** | Performance-focused playgrounds containing gamified database challenges. | [hackerrank.com/domains/sql](https://hackerrank.com) |
| **DataLemur** | Corporate style SQL problems tailored for analytics internships. | [datalemur.com](https://datalemur.com) |

---

## 🧪 Module Exercises — GROUP BY & HAVING

> **Instructions:** Write the SQL query for each exercise. Test your queries inside your pgAdmin SQL panel.

### Exercise 1
