# Module 15: Exercises — Indexing in PostgreSQL

> **Instructions:** Write the SQL for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance.

---

## 🟢 Easy Exercises

### Exercise 1
**Task:** Create an index on the `last_name` column of the `students` table to speed up name searches.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE INDEX idx_students_last_name ON students(last_name);
```
* **Explanation:** This sets up a standard B-tree index on the `last_name` column, enabling the database to avoid full table scans when looking up students by their surname.
</details>

---

### Exercise 2
**Task:** Create a unique index on the `email` column of the `students` table to enforce uniqueness and speed up email lookups.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE UNIQUE INDEX idx_students_email ON students(email);
```
* **Explanation:** A unique index accelerates search speeds while simultaneously enforcing data integrity by blocking any duplicate email entries.
</details>

---

### Exercise 3
**Task:** Safely drop the index `idx_students_last_name` if it exists.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
DROP INDEX IF EXISTS idx_students_last_name;
```
* **Explanation:** Adding the `IF EXISTS` clause prevents PostgreSQL from throwing an error if the index has already been dropped or does not exist.
</details>

---

### Exercise 4
**Task:** Run `EXPLAIN` on the following query and identify whether it uses a full table scan or an index scan (assume the index from Exercise 1 exists).
```sql
SELECT * FROM students WHERE last_name = 'Smith';
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
EXPLAIN SELECT * FROM students WHERE last_name = 'Smith';
```

#### Expected output (with index):
```text
Index Scan using idx_students_last_name on students
  Index Cond: (last_name = 'Smith'::text)
```

#### If no index exists, you would see:
```text
Seq Scan on students
  Filter: (last_name = 'Smith'::text)
```
* **Explanation:** `EXPLAIN` reveals the query planner's roadmap. An `Index Scan` points to fast lookup utilization, whereas a `Seq Scan` signifies a slower full table scan.
</details>

---

### Exercise 5
**Task:** Create an index on the `student_id` column of the `enrollments` table to speed up joins with the `students` table.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE INDEX idx_enrollments_student_id ON enrollments(student_id);
```
* **Explanation:** Indexing foreign keys like `student_id` inside mapping tables is a core production rule to heavily optimize `JOIN` execution routines.
</details>

---

## 🟡 Medium Exercises

### Exercise 6
**Task:** Create a multicolumn index on `enrollments(student_id, course_id)` to speed up queries that check for duplicate enrollments.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE INDEX idx_enrollments_student_course ON enrollments(student_id, course_id);
```
* **Explanation:** This indexes both parameters in a shared tree array, speeding up compound search filters targeting student registration anomalies.
</details>

---

### Exercise 7
**Task:** Create a partial index on `enrollments(student_id)` that only includes rows where `status = 'Active'`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE INDEX idx_active_enrollments ON enrollments(student_id)
WHERE status = 'Active';
```
* **Explanation:** By dropping historical or inactive data elements from the index map, a partial index stays remarkably lightweight and efficient.
</details>

---

### Exercise 8
**Task:** Write two queries: one to find all grades for `student_id = 5` using `EXPLAIN ANALYZE`. First, run it without an index on `grades(student_id)`. Then create the index and run it again. Compare the execution times.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
-- Step 1: Run without index
EXPLAIN ANALYZE
SELECT * FROM grades WHERE student_id = 5;

-- Step 2: Create the index
CREATE INDEX idx_grades_student_id ON grades(student_id);

-- Step 3: Run again with index
EXPLAIN ANALYZE
SELECT * FROM grades WHERE student_id = 5;
```
* **Expected observation:** The second execution report will showcase an `Index Scan` block instead of a `Seq Scan`, accompanied by a significantly lower execution time metric.
</details>

---

### Exercise 9
**Task:** Create an index that helps speed up the following query:
```sql
SELECT * FROM students ORDER BY last_name, first_name;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE INDEX idx_students_name_sort ON students(last_name, first_name);
```
* **Explanation:** Indexes naturally store structured records in predefined, sorted orders. This index completely eliminates the need for expensive in-memory sorting operations during query execution.
</details>

---

### Exercise 10
**Task:** A table already has `UNIQUE(email)` on the `students` table. Write a query to check if PostgreSQL automatically created an index for this constraint.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
\d students
-- Or query system tables:
SELECT indexname, indexdef
FROM pg_indexes
WHERE tablename = 'students';
```
* **Expected observation:** You will find an automated index record object named something like `students_email_key`. PostgreSQL deploys this underneath the hood to track uniqueness parameters implicitly.
</details>

---

## 🔴 Challenging Exercises

### Exercise 11
**Task:** The university registrar runs a dashboard with these frequent queries:
1. Find all students in a specific department.
2. Find all courses taught by a specific instructor.
3. Find all enrollments for a specific student in a specific semester.
4. Find all grades above 90 for a specific course.

Write `CREATE INDEX` statements for each query scenario.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
-- Query 1: Find all students in a specific department
CREATE INDEX idx_students_department ON students(department_id);

-- Query 2: Find all courses taught by a specific instructor
CREATE INDEX idx_courses_instructor ON courses(instructor_id);

-- Query 3: Find all enrollments for a specific student in a specific semester
CREATE INDEX idx_enrollments_student_semester ON enrollments(student_id, semester);

-- Query 4: Find all grades above 90 for a specific course
-- Index the foreign key linking field first
CREATE INDEX idx_grades_enrollment ON grades(enrollment_id);
-- Then optimize high score scans using a focused partial index
CREATE INDEX idx_high_grades ON grades(enrollment_id, numeric_grade)
WHERE numeric_grade > 90;
```
</details>

---

### Exercise 12
**Task:** A developer wants to create indexes on every column of the `students` table. Write a short paragraph (2–3 sentences) explaining why this is a bad idea, specifically mentioning `INSERT` and `UPDATE` performance.

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Sample Answer:** Indexing every single column slows down database writes significantly. Every time an application triggers an `INSERT` or `UPDATE` transaction, PostgreSQL must update the core physical row data while simultaneously rebuilding every single affected index map tree. Furthermore, columns that are rarely referenced in search filters end up wasting vast amounts of disk space without offering any practical optimization advantages.
</details>

---

### Exercise 13
**Task:** The following query is run very frequently:
```sql
SELECT student_id, last_name FROM students WHERE last_name = 'Smith';
```
Explain why an index on `last_name` alone is sufficient, but an index on `(last_name, student_id)` would be even better (a "covering index").

<details>
<summary><b>🔍 Show Answer</b></summary>

* **Explanation:** An index built solely on `last_name` pinpoints targeted rows efficiently but still forces PostgreSQL to perform an internal table lookup to retrieve the corresponding `student_id` field values. Upgrading to a compound index on `(last_name, student_id)` creates a **Covering Index**. This structure contains 100% of the data parameters requested by the query, allowing the database engine to fulfill the request entirely within the index tree scan without ever touching the underlying physical table files.
```sql
CREATE INDEX idx_students_covering ON students(last_name, student_id);
```
</details>

---

### Exercise 14
**Task:** Write a query against PostgreSQL system catalogs to find indexes on the `students` table that have never been scanned.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan
FROM pg_stat_user_indexes
WHERE tablename = 'students'
  AND idx_scan = 0;
```
* **Interpretation:** If the `idx_scan` metric stands at `0`, it means the index object has never been utilized by the system planner. You should safely consider dropping it in production environments to conserve disk write and processing overhead.
</details>

---
