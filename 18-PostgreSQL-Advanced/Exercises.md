# Module 18: Exercises — PostgreSQL Advanced Features

> **Instructions:** Write the SQL or explanation for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance.

---

## 🟢 Easy Exercises

### Exercise 1
**Task:** Write a CTE named `cs_students` that selects all students in `department_id = 1` (Computer Science). Then query the CTE to show their first and last names.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
WITH cs_students AS (
    SELECT student_id, first_name, last_name
    FROM students
    WHERE department_id = 1
)
SELECT first_name, last_name
FROM cs_students;
```
* **Explanation:** The `WITH` block encapsulates the restricted query as a temporary table structure named `cs_students`, which the outer main block selects from safely.
</details>

---

### Exercise 2
**Task:** Assign a unique row number to every student, ordered by GPA from highest to lowest. Show `first_name`, `last_name`, `gpa`, and the row number.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
    first_name,
    last_name,
    gpa,
    ROW_NUMBER() OVER (ORDER BY gpa DESC) AS overall_rank
FROM students;
```
* **Explanation:** `ROW_NUMBER()` assigns sequential integer ranks across the entire ordered table layout without collapsing any individual student records.
</details>

---

### Exercise 3
**Task:** Create a function named `get_department_name` that takes a `department_id` and returns the department's name.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE FUNCTION get_department_name(p_department_id INT)
RETURNS TEXT
LANGUAGE plpgsql
AS $$
DECLARE
    v_name TEXT;
BEGIN
    SELECT name INTO v_name
    FROM departments
    WHERE department_id = p_department_id;

    RETURN v_name;
END;
$$;
```

#### Usage:
```sql
SELECT get_department_name(1);
```
* **Explanation:** This defines a reusable PL/pgSQL function block that stores input lookups using `INTO` variables and pushes data back via the `RETURN` keyword.
</details>

---

### Exercise 4
**Task:** Create a procedure named `enroll_student` that takes `student_id` and `course_id`, and inserts a new enrollment for semester 'Fall 2025'.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE PROCEDURE enroll_student(
    p_student_id INT,
    p_course_id INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO enrollments (student_id, course_id, semester)
    VALUES (p_student_id, p_course_id, 'Fall 2025');
END;
$$;
```

#### Usage:
```sql
CALL enroll_student(1, 101);
```
* **Explanation:** Stored procedures do not declare output type fields, focus on performing data mutation steps, and must be triggered explicitly with the `CALL` command.
</details>

---

### Exercise 5
**Task:** Create a trigger that automatically sets `updated_at = CURRENT_TIMESTAMP` whenever a row in the `students` table is updated. Assume the `updated_at` column already exists.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
-- Step 1: Create the trigger function object
CREATE FUNCTION update_student_timestamp()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$;

-- Step 2: Create and bind the formal trigger constraint
CREATE TRIGGER trigger_student_updated_at
BEFORE UPDATE ON students
FOR EACH ROW
EXECUTE FUNCTION update_student_timestamp();
```
* **Explanation:** The `BEFORE UPDATE` block captures updates before they are committed, modifies the `NEW` data state automatically, and returns the modified row buffer.
</details>

---

## 🟡 Medium Exercises

### Exercise 6
**Task:** Write a CTE named `course_enrollments` that counts enrollments per course. Then join it with `courses` to show the course title and enrollment count.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
WITH course_enrollments AS (
    SELECT
        course_id,
        COUNT(*) AS enrollment_count
    FROM enrollments
    GROUP BY course_id
)
SELECT
    c.title AS course_title,
    COALESCE(ce.enrollment_count, 0) AS enrollment_count
FROM courses c
LEFT JOIN course_enrollments ce ON c.course_id = ce.course_id;
```
* **Explanation:** This pre-aggregates class registrations securely inside a named CTE window, and maps them to courses using `COALESCE` to turn missing inputs into a clean `0`.
</details>

---

### Exercise 7
**Task:** Rank students by GPA within each department. Show `first_name`, `last_name`, `department_id`, `gpa`, and their rank within the department.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
    first_name,
    last_name,
    department_id,
    gpa,
    RANK() OVER (
        PARTITION BY department_id
        ORDER BY gpa DESC
    ) AS dept_rank
FROM students;
```
* **Explanation:** The `PARTITION BY` parameter isolates calculation windows per distinct department department block, ranking student scores independently without dropping lines.
</details>

---

### Exercise 8
**Task:** Create a function named `get_student_courses` that takes a `student_id` and returns a table with columns `course_title` and `semester`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE FUNCTION get_student_courses(p_student_id INT)
RETURNS TABLE (
    course_title TEXT,
    semester VARCHAR(20)
)
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN QUERY
    SELECT c.title, e.semester
    FROM enrollments e
    JOIN courses c ON e.course_id = c.course_id
    WHERE e.student_id = p_student_id;
END;
$$;
```

#### Usage:
```sql
SELECT * FROM get_student_courses(1);
```
* **Explanation:** Functions can push back entire datasets using the `RETURNS TABLE` syntax combined with internal `RETURN QUERY` select execution tracers.
</details>

---

### Exercise 9
**Task:** Create a trigger that inserts a row into an `audit_log` table whenever a student's GPA is updated. The `audit_log` table has columns: `table_name`, `record_id`, `old_value`, `new_value`, `changed_at`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
-- First, establish the audit logging table structure
CREATE TABLE IF NOT EXISTS audit_log (
    log_id SERIAL PRIMARY KEY,
    table_name TEXT,
    record_id INT,
    old_value TEXT,
    new_value TEXT,
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create the trigger auditing function object
CREATE FUNCTION log_gpa_change()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    IF OLD.gpa IS DISTINCT FROM NEW.gpa THEN
        INSERT INTO audit_log (table_name, record_id, old_value, new_value)
        VALUES ('students', OLD.student_id, OLD.gpa::TEXT, NEW.gpa::TEXT);
    END IF;
    RETURN NEW;
END;
$$;

-- Create the final active table event trigger constraint link
CREATE TRIGGER trigger_log_gpa_change
AFTER UPDATE ON students
FOR EACH ROW
EXECUTE FUNCTION log_gpa_change();
```
* **Explanation:** Using the `IS DISTINCT FROM` clause captures deviations smoothly (even handling NULL cells), allowing you to write tracking rows into your audit trails.
</details>

---

### Exercise 10
**Task:** Write a query that shows `first_name`, `last_name`, `gpa`, and both `RANK()` and `DENSE_RANK()` ordered by GPA descending. Explain the difference in the results.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
    first_name,
    last_name,
    gpa,
    RANK() OVER (ORDER BY gpa DESC) AS rank_with_gaps,
    DENSE_RANK() OVER (ORDER BY gpa DESC) AS rank_no_gaps
FROM students;
```

#### Explanation:
> 🗣️ **ANALYSIS:** If two separate rows tie for 2nd place simultaneously:
> * `RANK()` assigns an identical `2` marker to both tied records, but skips positions to leave a gap, assigning a `4` to the next row sequence (1, 2, 2, 4).
> * `DENSE_RANK()` handles the tie similarly by giving both records a `2`, but moves directly to the next continuous value without gaps, assigning a `3` to the next row sequence (1, 2, 2, 3).
</details>

---

## 🔴 Challenging Exercises

### Exercise 11
**Task:** Write a query with TWO CTEs:
1. `department_stats`: counts students and average GPA per department.
2. `top_departments`: selects departments with average GPA above 3.0.
*Then join top_departments with departments to show the department name, student count, and average GPA.*

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
WITH department_stats AS (
    SELECT
        department_id,
        COUNT(*) AS student_count,
        AVG(gpa) AS avg_gpa
    FROM students
    GROUP BY department_id
),
top_departments AS (
    SELECT *
    FROM department_stats
    WHERE avg_gpa > 3.0
)
SELECT
    d.name AS department_name,
    td.student_count,
    ROUND(td.avg_gpa, 2) AS avg_gpa
FROM top_departments td
JOIN departments d ON td.department_id = d.department_id;
```
* **Explanation:** Multiple CTE code windows can be chained together inside a single statement by separating named blocks with commas after the initial `WITH` keyword.
</details>

---

### Exercise 12
**Task:** Using a window function, show each enrollment's `enrollment_id`, `course_id`, `student_id`, and a running total of how many students have enrolled in that course up to and including the current row. Order by `enrollment_id`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT
    enrollment_id,
    course_id,
    student_id,
    COUNT(*) OVER (
