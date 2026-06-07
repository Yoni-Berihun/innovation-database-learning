# Module 10: Exercises — SQL Joins

> **Instructions:** Write the SQL query for each exercise. Use the University Management System schema provided in [README.md](README.md). Test your queries in a local PostgreSQL instance or on [pgExercises.com](https://pgexercises.com/).

---

## 🟢 Easy Exercises

### Exercise 10.1: Basic INNER JOIN
**Task:** Write a query to list all instructors and the names of the departments they belong to.

**Expected Columns:** `instructor_first_name`, `instructor_last_name`, `department_name`

<details>
<summary>💡 Hint</summary>
Join the <code>instructors</code> table with the <code>departments</code> table on <code>department_id</code>.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  i.first_name AS instructor_first_name,
  i.last_name AS instructor_last_name,
  d.name AS department_name
FROM instructors i
INNER JOIN departments d
  ON i.department_id = d.department_id;
```
* **Explanation:** An `INNER JOIN` matches rows with identical `department_id` values. Using column aliases clarifies the output names.
</details>

---

### Exercise 10.2: Simple LEFT JOIN
**Task:** Write a query to list all courses and the instructor assigned to each course. If a course has no instructor, the instructor fields should show `NULL`.

**Expected Columns:** `course_title`, `instructor_first_name`, `instructor_last_name`

<details>
<summary>💡 Hint</summary>
Use <code>courses</code> as the left table and <code>LEFT JOIN</code> with <code>instructors</code>.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  c.title AS course_title,
  i.first_name AS instructor_first_name,
  i.last_name AS instructor_last_name
FROM courses c
LEFT JOIN instructors i
  ON c.instructor_id = i.instructor_id;
```
* **Explanation:** A `LEFT JOIN` preserves all rows from `courses` even if the `instructor_id` value is empty or has no match in the `instructors` table.
</details>

---

### Exercise 10.3: Join with Filtering
**Task:** Write a query to find all students who are enrolled in the course "Intro to CS". Show the student's first and last name.

**Expected Columns:** `first_name`, `last_name`

<details>
<summary>💡 Hint</summary>
You need to join <code>students</code> → <code>enrollments</code> → <code>courses</code>, then filter with <code>WHERE</code>.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  s.first_name,
  s.last_name
FROM students s
INNER JOIN enrollments e
  ON s.student_id = e.student_id
INNER JOIN courses c
  ON e.course_id = c.course_id
WHERE c.title = 'Intro to CS';
```
* **Explanation:** This is a multi-table join bridging `students` to `courses` through `enrollments`, with a `WHERE` condition isolating the specific course string.
</details>

---

## 🟡 Medium Exercises

### Exercise 10.4: Multiple Joins
**Task:** Write a query to display a full grade report: student's first/last name, course title, and numeric grade. Only show students who have grades.

**Expected Columns:** `first_name`, `last_name`, `course_title`, `numeric_grade`

<details>
<summary>💡 Hint</summary>
Join <code>students</code> → <code>enrollments</code> → <code>grades</code> → <code>courses</code>. Use <code>INNER JOIN</code> throughout.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  s.first_name,
  s.last_name,
  c.title AS course_title,
  g.numeric_grade
FROM students s
INNER JOIN enrollments e
  ON s.student_id = e.student_id
INNER JOIN grades g
  ON e.enrollment_id = g.enrollment_id
INNER JOIN courses c
  ON e.course_id = c.course_id;
```
* **Explanation:** Chaining multiple `INNER JOIN` clauses ensures that records are returned only if matching entries exist all the way through the sequence.
</details>

---

### Exercise 10.5: Finding Orphans with LEFT JOIN
**Task:** Write a query to find all students who have **not** enrolled in any courses.

**Expected Columns:** `student_id`, `first_name`, `last_name`

<details>
<summary>💡 Hint</summary>
Use a <code>LEFT JOIN</code> from <code>students</code> to <code>enrollments</code>, then filter where <code>enrollment_id IS NULL</code>.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  s.student_id,
  s.first_name,
  s.last_name
FROM students s
LEFT JOIN enrollments e
  ON s.student_id = e.student_id
WHERE e.enrollment_id IS NULL;
```
* **Explanation:** The `LEFT JOIN` returns all students. The `WHERE e.enrollment_id IS NULL` filter eliminates matches, isolating rows where no link exists.
</details>

---

### Exercise 10.6: Aggregation with Joins
**Task:** Write a query to count how many students are enrolled in each course. Show the course title and the student count. Include courses with zero students.

**Expected Columns:** `course_title`, `student_count`

<details>
<summary>💡 Hint</summary>
Use <code>LEFT JOIN</code> from <code>courses</code> to <code>enrollments</code>, then use <code>COUNT(e.student_id)</code> and <code>GROUP BY</code>.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  c.title AS course_title,
  COUNT(e.student_id) AS student_count
FROM courses c
LEFT JOIN enrollments e
  ON c.course_id = e.course_id
GROUP BY c.course_id, c.title;
```
* **Explanation:** We use a `LEFT JOIN` to preserve empty courses, and `COUNT(e.student_id)` to ensure courses with zero registrations return `0` instead of `1` (which `COUNT(*)` would do).
</details>

---

## 🔴 Challenging Exercises

### Exercise 10.7: Complex Multi-Table Report
**Task:** Write a query to produce a complete academic report showing:
* Student's full name
* Department name
* Course title
* Instructor's full name
* Letter grade

Only include rows where a grade actually exists.

**Expected Columns:** `student_name`, `department_name`, `course_title`, `instructor_name`, `letter_grade`

<details>
<summary>💡 Hint</summary>
You will need to join all six tables. Use aliases to keep the query readable.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  s.first_name || ' ' || s.last_name AS student_name,
  d.name AS department_name,
  c.title AS course_title,
  i.first_name || ' ' || i.last_name AS instructor_name,
  g.letter_grade
FROM students s
INNER JOIN enrollments e
  ON s.student_id = e.student_id
INNER JOIN grades g
  ON e.enrollment_id = g.enrollment_id
INNER JOIN courses c
  ON e.course_id = c.course_id
LEFT JOIN instructors i
  ON c.instructor_id = i.instructor_id
LEFT JOIN departments d
  ON s.department_id = d.department_id;
```
* **Explanation:** This comprehensive script links all database assets. The standard concatenation operator `||` merges first and last names together.
</details>

---

### Exercise 10.8: FULL JOIN Audit
**Task:** Write a query using `FULL JOIN` to find:
1. All instructors who do not belong to any department.
2. All departments that have no instructors.

**Expected Columns:** `instructor_name`, `department_name`

<details>
<summary>💡 Hint</summary>
Use <code>FULL JOIN</code> between <code>instructors</code> and <code>departments</code>. Look for rows where either side is <code>NULL</code>.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  i.first_name || ' ' || i.last_name AS instructor_name,
  d.name AS department_name
FROM instructors i
FULL JOIN departments d
  ON i.department_id = d.department_id
WHERE i.instructor_id IS NULL
   OR d.department_id IS NULL;
```
* **Explanation:** A `FULL JOIN` returns unmatched rows from both tables. The `WHERE` clause keeps rows where one of the matching primary identifiers is missing.
</details>

---

### Exercise 10.9: Self-Join Challenge
**Task:** (Conceptual Extension) Imagine a `prerequisites` table that links `courses` to other `courses` (e.g., course 102 requires course 101). Write a query to list each course and the title of its prerequisite course.

**Schema:**
```sql
CREATE TABLE prerequisites (
  course_id INT,
  prerequisite_course_id INT
);
```

<details>
<summary>💡 Hint</summary>
Join the <code>courses</code> table twice using distinct aliases (e.g., <code>c1</code> and <code>c2</code>) connected via the <code>prerequisites</code> table.
</details>

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
  c1.title AS course_title,
  c2.title AS prerequisite_title
FROM courses c1
INNER JOIN prerequisites p
  ON c1.course_id = p.course_id
INNER JOIN courses c2
  ON p.prerequisite_course_id = c2.course_id;
```
* **Explanation:** To reference identical schemas differently within one transaction, you split the main entity using different table aliases (`c1` and `c2`).
</details>
