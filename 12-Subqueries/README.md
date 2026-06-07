# 🧩 Subqueries in PostgreSQL

&gt; **Module 12** | **Skill Level:** Beginner | **Prerequisite:** Modules 10–11 (Joins, Grouping & HAVING)  
&gt; **Estimated Time:** 3–4 hours

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. Explain what a subquery is and why it is useful.
2. Write subqueries inside a `WHERE` clause to filter data dynamically.
3. Use `IN` with subqueries to match against a list of values.
4. Use subqueries in the `FROM` clause to create temporary result sets.
5. Understand the difference between subqueries and joins.
6. Identify and fix common beginner mistakes with subqueries.
7. Apply subqueries to real-world university management scenarios.

---

## 🧠 Why Subqueries Matter

In real-world applications, data is constantly changing. You rarely know exact values in advance. Subqueries let you ask PostgreSQL to **calculate a value first**, then use that result in your main query.

### University Examples
- **Finding students above average GPA:** You don't know the average GPA. Let PostgreSQL calculate it for you.
- **Finding courses with the highest enrollment:** The number changes every semester.
- **Filtering based on calculated data:** "Show me all students in departments with more than 50 students."

### How Companies Use Subqueries
- **Banking systems:** Find customers with balances above the average savings account balance.
- **E-commerce analytics:** Find products priced below the average in their category.
- **Recommendation systems:** "Show me movies rated higher than the average rating for their genre."
- **Reporting dashboards:** Generate reports that compare current sales to last year's sales without hard-coding numbers.

&gt; 💡 **Internship Tip:** Interviewers love subquery questions because they test whether you can think in steps, not just memorize syntax.

---

## 📖 What is a Subquery?

A **subquery** is a query inside another query.

Think of it like asking a helper question before answering the main question.

&gt; 🏫 **Analogy:** Imagine you are a university advisor. A student asks, "Am I doing better than average?" You can't answer immediately. You first need to ask a helper question: "What is the average GPA of all students?" Once you have that number, you answer the main question: "Is this student's GPA higher than that average?"

A subquery does exactly this. PostgreSQL runs the inner query first, gets the result, and then uses that result in the outer query.

---

## 📖 Basic Subquery Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE column1 &gt; (
    SELECT AVG(column1)
    FROM table_name
);


## Line-by-Line Explanation




| Line | What It Does |
| :--- | :--- |
| `SELECT column1, column2` | The outer query picks what to show. |
| `FROM table_name` | The outer query picks the main table. |
| `WHERE column1 > (...)` | The outer query filters rows. |
| `(SELECT AVG(column1) FROM table_name)` | The subquery calculates the average first. |

### Example: Students Above Average GPA
```sql
SELECT first_name, last_name, gpa
FROM students
WHERE gpa > (
    SELECT AVG(gpa)
    FROM students
);
```

#### What happens:
1. PostgreSQL runs the subquery: `SELECT AVG(gpa) FROM students` → result is, say, `3.2`.
2. PostgreSQL runs the outer query: `SELECT ... FROM students WHERE gpa > 3.2`.
3. You see only students with a GPA above 3.2.

> ⚠️ **Rule:** A subquery inside `WHERE` must return exactly one column and one row when used with comparison operators like `>`, `<`, `=`.

---

## 📖 Subqueries in WHERE

Subqueries in the `WHERE` clause are the most common type. They let you filter rows based on a calculated value.

### Example 1: Find Students in the Computer Science Department
```sql
SELECT first_name, last_name
FROM students
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE name = 'Computer Science'
);
```
#### What happens:
* The subquery finds the `department_id` for "Computer Science" (let's say it is `1`).
* The outer query finds all students where `department_id = 1`.

### Example 2: Find Courses Taught by Dr. Turing
```sql
SELECT title, credits
FROM courses
WHERE instructor_id = (
    SELECT instructor_id
    FROM instructors
    WHERE first_name = 'Dr. Alan'
      AND last_name = 'Turing'
);
```
#### What happens:
* The subquery finds Dr. Turing's `instructor_id`.
* The outer query finds all courses with that `instructor_id`.

---

## 📖 Subqueries with IN

Sometimes a subquery returns multiple rows. You cannot use `=` with multiple rows. Instead, you use `IN`. `IN` checks if a value matches any value in the subquery result.

### Example: Find All Students in Science Departments
```sql
SELECT first_name, last_name
FROM students
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE name LIKE '%Science%'
);
```
#### What happens:
* The subquery returns all `department_id` values where the name contains "Science" (e.g., `1` for Computer Science, `3` for Data Science).
* The outer query finds students whose `department_id` is `1` or `3`.

### Example: Find Students Enrolled in Any Course
```sql
SELECT first_name, last_name
FROM students
WHERE student_id IN (
    SELECT student_id
    FROM enrollments
);
```
#### What happens:
* The subquery returns a list of all `student_id` values found in `enrollments`.
* The outer query returns students whose ID appears in that list.

💡 `IN` is like asking: *"Is this student ID anywhere in the enrollment list?"*

---

## 📖 Subqueries in FROM

You can also use a subquery as a temporary table inside the `FROM` clause. This is useful when you need to calculate something first, then query those results.

### Example: Average GPA by Department
```sql
SELECT d.name, avg_gpa.avg_gpa_value
FROM departments d
JOIN (
    SELECT department_id, AVG(gpa) AS avg_gpa_value
    FROM students
    GROUP BY department_id
) avg_gpa
ON d.department_id = avg_gpa.department_id;
```
#### What happens:
* The subquery calculates the average GPA for each department.
* The outer query joins that result to the `departments` table to get the department names.

💡 Think of the subquery in `FROM` as creating a "mini table" on the fly. You **must** give it an alias (like `avg_gpa` above).

---

## 📖 Nested Queries

A nested query simply means a subquery inside another subquery. For beginners, we keep this simple.

### Example: Find the Student with the Highest GPA in Computer Science
```sql
SELECT first_name, last_name, gpa
FROM students
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE name = 'Computer Science'
)
AND gpa = (
    SELECT MAX(gpa)
    FROM students
    WHERE department_id = (
        SELECT department_id
        FROM departments
        WHERE name = 'Computer Science'
    )
);
```
#### What happens:
1. The first subquery finds the Computer Science `department_id`.
2. The second subquery finds the maximum GPA among students in that department.
3. The outer query finds the student who matches both conditions.

> ⚠️ **Beginner Tip:** Deep nesting can get confusing. If you find yourself with more than two levels of subqueries, consider rewriting with a `JOIN`. But understanding nesting is important for reading other people's code.

---

## 🌍 Real-World Examples

### Example 1: Students Above Average GPA
```sql
SELECT first_name, last_name, gpa
FROM students
WHERE gpa > (
    SELECT AVG(gpa)
    FROM students
);
```
* **Practical Relevance:** The registrar needs a dean's list. The average GPA changes every semester. A subquery ensures the list is always accurate without manual updates.

### Example 2: Most Enrolled Courses
```sql
SELECT c.title, COUNT(e.student_id) AS enrollment_count
FROM courses c
JOIN enrollments e ON c.course_id = e.course_id
GROUP BY c.course_id, c.title
HAVING COUNT(e.student_id) = (
    SELECT MAX(enrollment_count)
    FROM (
        SELECT COUNT(student_id) AS enrollment_count
        FROM enrollments
        GROUP BY course_id
    ) sub
);
```
* **Practical Relevance:** The scheduling office needs to know which courses are most popular to plan room assignments and add extra sections.

### Example 3: Departments with Many Students
```sql
SELECT name
FROM departments
WHERE department_id IN (
    SELECT department_id
    FROM students
    GROUP BY department_id
    HAVING COUNT(*) > 50
);
```
* **Practical Relevance:** The administration needs to allocate funding. Departments with more than 50 students get additional lab equipment.

---

## 💻 Hands-On Practice

Practice the following patterns using the University Management System schema:
* **WHERE subqueries:** Filter students based on calculated averages.
* **IN subqueries:** Find students in specific departments or enrolled in specific courses.
* **FROM subqueries:** Calculate averages first, then join to get names.

### Quick Practice Queries
```sql
-- Practice 1: Find instructors who teach at least one course
SELECT first_name, last_name
FROM instructors
WHERE instructor_id IN (
    SELECT instructor_id
    FROM courses
    WHERE instructor_id IS NOT NULL
);

-- Practice 2: Find the average grade for each course
SELECT c.title, avg_grade.avg_score
FROM courses c
JOIN (
    SELECT e.course_id, AVG(g.numeric_grade) AS avg_score
    FROM enrollments e
    JOIN grades g ON e.enrollment_id = g.enrollment_id
    GROUP BY e.course_id
) avg_grade
ON c.course_id = avg_grade.course_id;

-- Practice 3: Find students who have never enrolled
SELECT first_name, last_name
FROM students
WHERE student_id NOT IN (
    SELECT student_id
    FROM enrollments
);
```

---

## ⚠️ Common Beginner Mistakes




| Mistake | Example | Fix |
| :--- | :--- | :--- |
| **Forgetting parentheses** | `WHERE gpa > SELECT AVG(gpa)...` | Always wrap subqueries in parentheses: `(SELECT ...)` |
| **Subquery returns too many rows** | `WHERE gpa = (SELECT gpa...)` | Use `IN` instead of `=` if multiple rows are possible. |
| **Confusing joins vs subqueries** | Using a subquery when a `JOIN` is cleaner | Ask: *"Do I need a calculated value, or just matching rows?"* |
| **Missing alias for FROM subquery** | `FROM (SELECT ...) JOIN ...` | Always add an alias: `FROM (SELECT ...) AS alias_name` |
| **NULL values with NOT IN** | `WHERE id NOT IN (SELECT id...)` | If subquery returns `NULL`, `NOT IN` fails. Use `NOT EXISTS` or ensure no `NULL`s. |

---

## ✅ Quick Check Questions

1. What is a subquery?
2. Why would you use a subquery instead of hard-coding a value?
3. Where can subqueries be placed in a SQL statement?
4. What is the difference between a subquery and a `JOIN`?
5. What does the `IN` operator do when used with a subquery?
6. Why must a subquery in `FROM` have an alias?
7. What happens if a subquery returns multiple rows but you use `=` instead of `IN`?

---

## 🎯 Key Takeaways

* A subquery is a query nested inside another query.
* Subqueries let you calculate values dynamically instead of hard-coding numbers.
* Use subqueries in `WHERE` to filter based on calculated results.
* Use `IN` with subqueries when the inner query returns multiple rows.
* Use subqueries in `FROM` to create temporary calculated tables.
* Subqueries and `JOIN`s can often solve the same problem, but subqueries are better for single calculated values, while `JOIN`s are better for combining rows from multiple tables.
* Always wrap subqueries in parentheses.
* Always give subqueries in `FROM` an alias.
