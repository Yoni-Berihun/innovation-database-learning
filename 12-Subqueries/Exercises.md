#  Exercises — Subqueries in PostgreSQL

> **Instructions:** Write the SQL query for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance or on [SQLZoo](https://sqlzoo.net) / [pgExercises](https://pgexercises.com).

---

## 🟢 Easy Exercises

### Exercise 12.1: Basic WHERE Subquery
**Task:** Find all students whose GPA is higher than the average GPA of all students.

**Expected Columns:** `first_name`, `last_name`, `gpa`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT first_name, last_name, gpa
FROM students
WHERE gpa > (
    SELECT AVG(gpa)
    FROM students
);
```
* **Explanation:** The inner query calculates the global average GPA. The outer query filters and retains only the students whose individual GPA exceeds that calculated scalar value.
</details>

---

### Exercise 12.2: Subquery with IN
**Task:** Find all students who are enrolled in at least one course.

**Expected Columns:** `first_name`, `last_name`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT first_name, last_name
FROM students
WHERE student_id IN (
    SELECT DISTINCT student_id
    FROM enrollments
);
```
* **Explanation:** The subquery extracts a list of all student IDs present in the execution logs of the `enrollments` table. The `IN` operator matches the primary student records against that list.
</details>

---

### Exercise 12.3: Subquery in FROM
**Task:** Calculate the average GPA for each department. Show the department name and the average GPA.

**Expected Columns:** `department_name`, `average_gpa`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT d.name AS department_name, dept_avg.average_gpa
FROM departments d
JOIN (
    SELECT department_id, AVG(gpa) AS average_gpa
    FROM students
    GROUP BY department_id
) dept_avg
  ON d.department_id = dept_avg.department_id;
```
* **Explanation:** The inner query groups students by department to compute targeted averages, turning into a temporary table alias `dept_avg`. The outer query joins it with `departments` to fetch the plain names.
</details>

---

### Exercise 12.4: Simple Department Filter
**Task:** Find all courses taught by instructors in the "Computer Science" department.

**Expected Columns:** `course_title`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT title AS course_title
FROM courses
WHERE instructor_id IN (
    SELECT instructor_id
    FROM instructors
    WHERE department_id = (
        SELECT department_id
        FROM departments
        WHERE name = 'Computer Science'
    )
);
```
* **Explanation:** This layered subquery architecture maps the department name to an ID, collects all instructors assigned to that ID, and filters the `courses` catalog based on those teacher keys.
</details>

---

### Exercise 12.5: Enrollment Check
**Task:** Find all students who have NOT enrolled in any courses.

**Expected Columns:** `first_name`, `last_name`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT first_name, last_name
FROM students
WHERE student_id NOT IN (
    SELECT student_id
    FROM enrollments
    WHERE student_id IS NOT NULL
);
```
* **Explanation:** Isolates the active student records whose unique identification numbers cannot be found anywhere within the `enrollments` data logs. Filtering out potential `NULL`s prevents total query empty sets.
</details>

---

## 🟡 Medium Exercises

### Exercise 12.6: Above Department Average
**Task:** Find all students whose GPA is higher than the average GPA of their own department.

**Expected Columns:** `first_name`, `last_name`, `gpa`, `department_name`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT s.first_name, s.last_name, s.gpa, d.name AS department_name
FROM students s
JOIN departments d ON s.department_id = d.department_id
WHERE s.gpa > (
    SELECT AVG(sub.gpa)
    FROM students sub
    WHERE sub.department_id = s.department_id
);
```
* **Explanation:** This represents a **correlated subquery**. The inner query recalculates the targeted department mean dynamically for each row evaluated by the outer query execution pointer.
</details>

---

### Exercise 12.7: Popular Courses
**Task:** Find the course(s) with the highest number of enrolled students. Show the course title and the enrollment count.

**Expected Columns:** `course_title`, `enrollment_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT c.title AS course_title, COUNT(e.student_id) AS enrollment_count
FROM courses c
JOIN enrollments e ON c.course_id = e.course_id
GROUP BY c.course_id, c.title
HAVING COUNT(e.student_id) = (
    SELECT MAX(sub.cnt)
    FROM (
        SELECT COUNT(student_id) AS cnt
        FROM enrollments
        GROUP BY course_id
    ) sub
);
```
* **Explanation:** Calculates explicit enrollment volumes grouped by class, and evaluates results inside a `HAVING` clause against a subquery calculating the absolute peak enrollment count value.
</details>

---

### Exercise 12.8: Instructor Workload
**Task:** Find all instructors who teach more courses than the average number of courses taught per instructor.

**Expected Columns:** `first_name`, `last_name`, `course_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT i.first_name, i.last_name, COUNT(c.course_id) AS course_count
FROM instructors i
JOIN courses c ON i.instructor_id = c.instructor_id
GROUP BY i.instructor_id, i.first_name, i.last_name
HAVING COUNT(c.course_id) > (
    SELECT AVG(sub.cnt)
    FROM (
        SELECT COUNT(course_id) AS cnt
        FROM courses
        WHERE instructor_id IS NOT NULL
        GROUP BY instructor_id
    ) sub
);
```
* **Explanation:** The subquery computes the mean course load value of active instructors. The outer layer groups and retains teachers whose personal output counts scale above that index baseline.
</details>

---

### Exercise 12.9: Grade Comparison
**Task:** Find all students who received a grade higher than the average grade for the course they took. Show the student's name, course title, and their grade.

**Expected Columns:** `student_name`, `course_title`, `numeric_grade`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT s.first_name || ' ' || s.last_name AS student_name, co.title AS course_title, g.numeric_grade
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN grades g ON e.enrollment_id = g.enrollment_id
JOIN courses co ON e.course_id = co.course_id
WHERE g.numeric_grade > (
    SELECT AVG(sub_g.numeric_grade)
    FROM enrollments sub_e
    JOIN grades sub_g ON sub_e.enrollment_id = sub_g.enrollment_id
    WHERE sub_e.course_id = e.course_id
);
```
* **Explanation:** A correlated subquery isolates class metrics for the specific line token, processing average course raw scores and evaluating individual matching keys.
</details>

---

### Exercise 12.10: Department Size Filter
**Task:** Find all departments that have more than 2 students. Show the department name and the student count.

**Expected Columns:** `department_name`, `student_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT d.name AS department_name, dept_counts.student_count
FROM departments d
JOIN (
    SELECT department_id, COUNT(*) AS student_count
    FROM students
    GROUP BY department_id
) dept_counts
  ON d.department_id = dept_counts.department_id
WHERE dept_counts.student_count > 2;
```
* **Explanation:** Summarizes student headcount volumes cleanly as a temporary dataset object inside the `FROM` loop, and enforces filtering via the outer scope layer.
</details>

---

## 🔴 Challenging Exercises

### Exercise 12.11: Top Performer Per Department
**Task:** Find the student with the highest GPA in each department. Show the department name, student name, and GPA. If two students tie, show both.

**Expected Columns:** `department_name`, `student_name`, `gpa`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT d.name AS department_name, s.first_name || ' ' || s.last_name AS student_name, s.gpa
FROM students s
JOIN departments d ON s.department_id = d.department_id
WHERE s.gpa = (
    SELECT MAX(sub.gpa)
    FROM students sub
    WHERE sub.department_id = s.department_id
);
```
* **Explanation:** Uses a correlated subquery targeting the localized ceiling aggregate value (`MAX(gpa)`). Rows that match that specific maximum flag are kept, gracefully handling ties.
</details>

---

### Exercise 12.12: Unassigned Instructors
**Task:** Find all instructors who are not currently teaching any courses AND are not assigned to any department.

**Expected Columns:** `first_name`, `last_name`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT first_name, last_name
FROM instructors
WHERE instructor_id NOT IN (
    SELECT DISTINCT instructor_id
    FROM courses
    WHERE instructor_id IS NOT NULL
) AND department_id IS NULL;
```
* **Explanation:** Combines subquery row exclusion metrics with static `NULL` testing checks to trap edge data points matching both systemic operational criteria simultaneously.
</details>

---

### Exercise 12.13: Course Recommendation Engine
**Task:** Find all students who are enrolled in "Intro to CS" but are NOT enrolled in "Database Systems". These students might be recommended to take Database Systems next.

**Expected Columns:** `student_name`

<details>
