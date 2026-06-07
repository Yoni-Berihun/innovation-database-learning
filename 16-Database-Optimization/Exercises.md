# Module 16: Exercises — Database Optimization in PostgreSQL

> **Instructions:** Write the SQL or explanation for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance.

---

## 🟢 Easy Exercises

### Exercise 1
**Task:** Rewrite the following inefficient query to only fetch the columns actually needed for a student directory page (student ID, first name, last name, email).

```sql
SELECT * FROM students;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT student_id, first_name, last_name, email
FROM students;
```
* **Explanation:** Avoiding `SELECT *` reduces disk I/O, saves network bandwidth, and prevents unnecessary memory usage in both the database engine and the application layer.
</details>

---

### Exercise 2
**Task:** Rewrite the following query to filter at the database level instead of fetching all rows.
```sql
SELECT * FROM enrollments;
-- Application code filters for semester = 'Fall 2025'
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT enrollment_id, student_id, course_id, semester
FROM enrollments
WHERE semester = 'Fall 2025';
```
* **Explanation:** Filtering at the database level using a `WHERE` clause allows PostgreSQL to discard unwanted rows immediately, drastically cutting down transmission payload sizes.
</details>

---

### Exercise 3
**Task:** Write a query to show the first 20 students, ordered by last name, for page 1 of a student directory.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT student_id, first_name, last_name, email
FROM students
ORDER BY last_name, first_name
LIMIT 20 OFFSET 0;
```
* **Explanation:** Enforcing `LIMIT` restricts the size of the result set, making it highly suitable for UI pagination and preventing database memory exhaustion.
</details>

---

### Exercise 4
**Task:** You run `EXPLAIN` on a query and see `Seq Scan on students`. What does this mean, and what is one way to speed it up?

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Explanation:** A `Seq Scan` (Sequential Scan) means PostgreSQL is reading every single row in the `students` table from top to bottom on disk. This becomes critically slow as the table grows.
* **How to speed it up:** Build a B-tree index on the specific column used inside the `WHERE` clause filter. For example, if the query filters by `last_name`:
```sql
CREATE INDEX idx_students_last_name ON students(last_name);
```
</details>

---

### Exercise 5
**Task:** A developer wants to store a student's GPA. They are considering `TEXT` and `NUMERIC(3,2)`. Which should they choose and why?

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Answer:** They should explicitly choose **`NUMERIC(3,2)`**.
* **Reasons:**
  1. `NUMERIC(3,2)` guarantees exact decimal arithmetic, making math operations like averages (`AVG`) or sorting filters accurate.
  2. `TEXT` stores numerical data as plain string characters, preventing any direct calculation blocks without forcing expensive type-casting routines.
  3. `NUMERIC(3,2)` occupies much less physical storage space and indexes significantly faster than unbounded text fields.
</details>

---

## 🟡 Medium Exercises

### Exercise 6
**Task:** The following query is slow. Use `EXPLAIN` to identify the problem, then add the appropriate index.
```sql
SELECT s.first_name, s.last_name, c.title
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id
WHERE s.last_name = 'Smith';
```

<details>
<summary><b>🔍 Show Answer</b></summary>

#### Step 1: Analyze with EXPLAIN
```sql
EXPLAIN SELECT s.first_name, s.last_name, c.title
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id
WHERE s.last_name = 'Smith';
```
* **Identified Problem:** The output will reveal a `Seq Scan on students s` with a filter evaluating `last_name`. 

#### Step 2: Build the appropriate index
```sql
CREATE INDEX idx_students_last_name ON students(last_name);
```

#### Step 3: Verify the adjustment
```sql
EXPLAIN ANALYZE SELECT ...;
-- The planning trace will now reflect a fast 'Index Scan' using idx_students_last_name.
```
</details>

---

### Exercise 7
**Task:** Rewrite the following subquery as a `JOIN` for better readability and potentially better performance.
```sql
SELECT *
FROM students
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE name = 'Computer Science'
);
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT s.*
FROM students s
JOIN departments d ON s.department_id = d.department_id
WHERE d.name = 'Computer Science';
```
* **Explanation:** Converting independent subqueries into explicit `JOIN` operations makes scripts easier to read and allows the query optimizer more structural flexibility to choose better search paths.
</details>

---

### Exercise 8
**Task:** The following pseudocode has an N+1 query problem. Rewrite it as a single SQL query.
```python
# Bad: N+1 queries loop
students = db.query("SELECT * FROM students")

for student in students:
    courses = db.query(f"""
        SELECT c.title
        FROM enrollments e
        JOIN courses c ON e.course_id = c.course_id
        WHERE e.student_id = {student['student_id']}
    """)
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
    s.student_id,
    s.first_name,
    s.last_name,
    c.title AS course_title
FROM students s
LEFT JOIN enrollments e ON s.student_id = e.student_id
LEFT JOIN courses c ON e.course_id = c.course_id;
```
* **Explanation:** This single consolidated schema link pulls all target columns simultaneously in one single round-trip database transaction, eliminating the N+1 code loop bottleneck completely.
</details>

---

### Exercise 9
**Task:** After importing 10,000 new student records, write the command to clean up dead rows and update statistics for the `students` table.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
VACUUM ANALYZE students;
```
* **Explanation:** `VACUUM` cleans out dead space leftover from internal multi-version transactional loops, while `ANALYZE` forces a complete statistical scan update so the query planner can accurately select optimal scanning paths.
</details>

---

### Exercise 10
**Task:** The following query accidentally creates a Cartesian product. Fix it by adding proper `JOIN` conditions.
```sql
SELECT s.first_name, c.title
FROM students s, courses c;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT s.first_name, c.title
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id;
```
* **Explanation:** The original syntax missed relational linkage hooks, forcing the engine to multiply every student against every single catalog course row. The modified code maps relationships accurately through the `enrollments` bridging entity.
</details>

---

## 🔴 Challenging Exercises

### Exercise 11
**Task:** You have two queries that produce the same result. Compare their execution plans and explain which is better.

#### Query A:
```sql
SELECT s.first_name, s.last_name
FROM students s
WHERE s.student_id IN (
    SELECT e.student_id
    FROM enrollments e
    WHERE e.course_id = 101
);
```

#### Query B:
```sql
SELECT s.first_name, s.last_name
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
WHERE e.course_id = 101;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Analysis:** Modern versions of PostgreSQL deploy smart rewrite algorithms that optimize both structures into very similar physical execution plans (often utilizing semi-join mutations).
* **Which is better:** **Query B** is generally preferred by engineering teams. Explicit `JOIN` operators offer cleaner legibility profiles, give the internal optimizer expanded path evaluation choices, and make expanding the query (such as fetching transactional fields out of `enrollments`) much easier.
</details>

---

### Exercise 12
**Task:** The registrar runs this report daily. It takes 15 seconds. Design indexes to speed it up.
```sql
SELECT
    d.name AS department_name,
    c.title AS course_title,
    COUNT(e.student_id) AS enrollment_count,
    AVG(g.numeric_grade) AS average_grade
FROM departments d
JOIN instructors i ON d.department_id = i.department_id
JOIN courses c ON i.instructor_id = c.instructor_id
LEFT JOIN enrollments e ON c.course_id = e.course_id
LEFT JOIN grades g ON e.enrollment_id = g.enrollment_id
WHERE e.semester = 'Fall 2025'
GROUP BY d.department_id, d.name, c.course_id, c.title;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
-- Speed up structural department links
CREATE INDEX idx_instructors_department ON instructors(department_id);

-- Speed up instructor assignment traversals
CREATE INDEX idx_courses_instructor ON courses(instructor_id);

-- Speed up student enrollment evaluations
