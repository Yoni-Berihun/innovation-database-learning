#  Exercises — Views in PostgreSQL

> **Instructions:** Write the SQL for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance or on [pgExercises](https://pgexercises.com) / [SQLZoo](https://sqlzoo.net).

---

## 🟢 Easy Exercises

### Exercise 1
**Task:** Create a view named `computer_science_students` that shows the `student_id`, `first_name`, `last_name`, and `email` of all students in the Computer Science department (assume `department_id = 1`).

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW computer_science_students AS
SELECT student_id, first_name, last_name, email
FROM students
WHERE department_id = 1;
```
* **Explanation:** This defines a basic virtual table by storing the `SELECT` query structure. It acts as a permanent shortcut for filtering student rows.
</details>

---

### Exercise 2
**Task:** Write a query to select all rows from the `computer_science_students` view where the `last_name` starts with 'J'.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT * FROM computer_science_students
WHERE last_name LIKE 'J%';
```
* **Explanation:** You can query a view exactly like a normal table. PostgreSQL combines this outer filter with the view's internal logic behind the scenes.
</details>

---

### Exercise 3
**Task:** Write the SQL to safely drop the `computer_science_students` view if it exists.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
DROP VIEW IF EXISTS computer_science_students;
```
* **Explanation:** The `IF EXISTS` clause prevents the database from throwing an error if the view has already been deleted or does not exist.
</details>

---

### Exercise 4
**Task:** Create a view named `student_directory` that shows `student_id`, `full_name` (concatenation of `first_name` and `last_name`), and `email`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW student_directory AS
SELECT
    student_id,
    first_name || ' ' || last_name AS full_name,
    email
FROM students;
```
* **Explanation:** Views can cleanly encapsulate string functions like the concatenation operator (`||`) and expose them as simple, standard table columns.
</details>

---

### Exercise 5
**Task:** Create a view named `high_gpa_students` that shows all students with a GPA of 3.5 or higher.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW high_gpa_students AS
SELECT *
FROM students
WHERE gpa >= 3.5;
```
* **Explanation:** This isolates high-achieving student rows dynamically, ensuring any changes to student grades are immediately reflected through this lens.
</details>

---

## 🟡 Medium Exercises

### Exercise 6
**Task:** Create a view named `enrollment_details` that shows `student_name`, `course_title`, and `semester`. Join `students`, `enrollments`, and `courses`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW enrollment_details AS
SELECT
    s.first_name || ' ' || s.last_name AS student_name,
    c.title AS course_title,
    e.semester
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id;
```
* **Explanation:** Views abstract away complex join pathways. Applications can now query `enrollment_details` directly without rewriting multi-table linkages.
</details>

---

### Exercise 7
**Task:** Create a view named `fall_2025_enrollments` that shows the same columns as `enrollment_details` but only for the 'Fall 2025' semester.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW fall_2025_enrollments AS
SELECT
    s.first_name || ' ' || s.last_name AS student_name,
    c.title AS course_title,
    e.semester
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id
WHERE e.semester = 'Fall 2025';
```
* **Explanation:** Combines multi-table physical joins with specific filter attributes to establish a localized operational dataset window.
</details>

---

### Exercise 8
**Task:** Create a view named `course_summary` that shows `course_id`, `course_title`, `instructor_name` (concatenation of first and last name), and `department_name`. Join `courses`, `instructors`, and `departments`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW course_summary AS
SELECT
    c.course_id,
    c.title AS course_title,
    i.first_name || ' ' || i.last_name AS instructor_name,
    d.name AS department_name
FROM courses c
LEFT JOIN instructors i ON c.instructor_id = i.instructor_id
LEFT JOIN departments d ON i.department_id = d.department_id;
```
* **Explanation:** Using `LEFT JOIN` operations inside the view definition block guarantees that courses remain visible even if an instructor or department is missing.
</details>

---

### Exercise 9
**Task:** Create a view named `public_student_info` that shows `student_id`, `first_name`, `last_name`, and `department_id`, but excludes `email`, `gpa`, and any other sensitive columns.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW public_student_info AS
SELECT student_id, first_name, last_name, department_id
FROM students;
```
* **Explanation:** This is a crucial database security technique. By granting query access to this view instead of the base table, you hide confidential student columns.
</details>

---

### Exercise 10
**Task:** Create a view named `students_with_grades` that shows `student_id`, `first_name`, `last_name`, and a boolean column `has_grades` that is `TRUE` if the student has at least one grade recorded.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW students_with_grades AS
SELECT
    s.student_id,
    s.first_name,
    s.last_name,
    EXISTS (
        SELECT 1
        FROM enrollments e
        JOIN grades g ON e.enrollment_id = g.enrollment_id
        WHERE e.student_id = s.student_id
    ) AS has_grades
FROM students s;
```
* **Explanation:** The `EXISTS` subquery acts as an inline flag, evaluating transaction logs row-by-row and outputting a clean logical boolean indicator.
</details>

---

## 🔴 Challenging Exercises

### Exercise 11
**Task:** Create a view named `department_student_counts` that shows `department_name` and `student_count` (number of students per department). Include departments with zero students.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW department_student_counts AS
SELECT
    d.name AS department_name,
    COUNT(s.student_id) AS student_count
FROM departments d
LEFT JOIN students s ON d.department_id = s.department_id
GROUP BY d.department_id, d.name;
```
* **Explanation:** Groups records and calculates metrics on the fly. Note that because this contains aggregate functions, it operates strictly as a read-only view.
</details>

---

### Exercise 12
**Task:** Create a view named `instructor_performance` that shows `instructor_name`, `course_count`, and `average_course_grade` (average of all numeric grades across all their courses). Only include instructors who teach at least one course.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW instructor_performance AS
SELECT
    i.first_name || ' ' || i.last_name AS instructor_name,
    COUNT(DISTINCT c.course_id) AS course_count,
    AVG(g.numeric_grade) AS average_course_grade
FROM instructors i
JOIN courses c ON i.instructor_id = c.instructor_id
JOIN enrollments e ON c.course_id = e.course_id
LEFT JOIN grades g ON e.enrollment_id = g.enrollment_id
GROUP BY i.instructor_id, i.first_name, i.last_name;
```
* **Explanation:** Employs multiple aggregations simultaneously. `COUNT(DISTINCT...)` maps unique elements precisely, while `AVG` handles numeric grades across multi-table joins.
</details>

---

### Exercise 13
**Task:** First, create a view named `active_enrollments` that shows all enrollments from the current year (assume 2025). Then, create a second view named `active_student_transcripts` that uses `active_enrollments` to show `student_name`, `course_title`, and `letter_grade`.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW active_enrollments AS
SELECT *
FROM enrollments
WHERE semester LIKE '%2025%';

CREATE VIEW active_student_transcripts AS
SELECT
    s.first_name || ' ' || s.last_name AS student_name,
    c.title AS course_title,
    g.letter_grade
FROM students s
JOIN active_enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id
LEFT JOIN grades g ON e.enrollment_id = g.enrollment_id;
```
* **Explanation:** Views can build on top of other views. Here, `active_student_transcripts` treats the virtual table `active_enrollments` as if it were a physical parent data file.
</details>

---

### Exercise 14
**Task:** Create an updatable view named `math_department_courses` that shows `course_id`, `title`, and `credits` for courses in the Mathematics department (assume `department_id = 2`). Then, write an `UPDATE` statement through this view to change the credits of course 201 to 4.

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
CREATE VIEW math_department_courses AS
SELECT course_id, title, credits
FROM courses
WHERE department_id = 2;

UPDATE math_department_courses
SET credits = 4
WHERE course_id = 201;
```
