# Module 11: Exercises — GROUP BY & HAVING in PostgreSQL

> **Instructions:** Write the SQL query for each exercise. Use the University Management System schema. Test your queries in a local PostgreSQL instance or on [SQLZoo](https://sqlzoo.net) / [pgExercises](https://pgexercises.com).

---

## 🟢 Easy Exercises

### Exercise 1: Basic GROUP BY
**Task:** Count how many students are in each department. Show the `department_id` and the student count.

**Expected Columns:** `department_id`, `student_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT department_id, COUNT(*) AS student_count
FROM students
GROUP BY department_id;
```
* **Explanation:** `GROUP BY department_id` bundles student rows into distinct clusters based on their department, and `COUNT(*)` calculates the total headcount within each cluster.
</details>

---

### Exercise 2: GROUP BY with AVG
**Task:** Calculate the average GPA for each department. Show the `department_id` and `average_gpa`.

**Expected Columns:** `department_id`, `average_gpa`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT department_id, AVG(gpa) AS average_gpa
FROM students
GROUP BY department_id;
```
* **Explanation:** The database creates groups for each unique `department_id` and computes the arithmetic mean of the `gpa` column for that group.
</details>

---

### Exercise 3: GROUP BY with COUNT and JOIN
**Task:** Count how many courses each instructor teaches. Show the instructor's full name and course count.

**Expected Columns:** `instructor_name`, `course_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT i.first_name || ' ' || i.last_name AS instructor_name, COUNT(c.course_id) AS course_count
FROM instructors i
INNER JOIN courses c ON i.instructor_id = c.instructor_id
GROUP BY i.instructor_id, i.first_name, i.last_name;
```
* **Explanation:** We join `instructors` and `courses` via `instructor_id`. The non-aggregated name columns must appear in the `GROUP BY` clause alongside the unique ID to ensure valid output mapping.
</details>

---

### Exercise 4: Simple HAVING Filter
**Task:** Find all departments that have more than 2 students. Show the `department_id` and student count.

**Expected Columns:** `department_id`, `student_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT department_id, COUNT(*) AS student_count
FROM students
GROUP BY department_id
HAVING COUNT(*) > 2;
```
* **Explanation:** `HAVING` acts as a post-group filter. It evaluates the computed `COUNT(*)` sizes and drops any groups that contain 2 or fewer student rows.
</details>

---

### Exercise 5: GROUP BY with SUM
**Task:** Calculate the total credits offered by each department. Show the `department_id` and `total_credits`.

**Expected Columns:** `department_id`, `total_credits`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT department_id, SUM(credits) AS total_credits
FROM courses
GROUP BY department_id;
```
* **Explanation:** This groups all course entries by their respective department ownership identifier and adds up the numbers in the `credits` column.
</details>

---

## 🟡 Medium Exercises

### Exercise 6: WHERE and HAVING Together
**Task:** Find departments where the average GPA of **active** students is above 3.0. Show the department name and average GPA.

**Expected Columns:** `department_name`, `average_gpa`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT d.name AS department_name, AVG(s.gpa) AS average_gpa
FROM students s
JOIN departments d ON s.department_id = d.department_id
WHERE s.status = 'Active'
GROUP BY d.department_id, d.name
HAVING AVG(s.gpa) > 3.0;
```
* **Explanation:** The `WHERE` clause filters row states *before* grouping occurs. `GROUP BY` organizes the remaining active students, and `HAVING` filters the resulting aggregate averages.
</details>

---

### Exercise 7: HAVING with Multiple Conditions
**Task:** Find instructors who teach at least 2 courses and whose courses have a total of at least 6 credits. Show the instructor name, course count, and total credits.

**Expected Columns:** `instructor_name`, `course_count`, `total_credits`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT i.first_name || ' ' || i.last_name AS instructor_name, 
       COUNT(c.course_id) AS course_count, 
       SUM(c.credits) AS total_credits
FROM instructors i
JOIN courses c ON i.instructor_id = c.instructor_id
GROUP BY i.instructor_id, i.first_name, i.last_name
HAVING COUNT(c.course_id) >= 2 AND SUM(c.credits) >= 6;
```
* **Explanation:** You can combine multiple logical conditions inside a `HAVING` clause using the `AND` operator to filter clusters against multiple calculated parameters.
</details>

---

### Exercise 8: GROUP BY with MIN and MAX
**Task:** For each course, find the lowest and highest numeric grades. Show the course title, minimum grade, and maximum grade.

**Expected Columns:** `course_title`, `min_grade`, `max_grade`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT c.title AS course_title, MIN(g.numeric_grade) AS min_grade, MAX(g.numeric_grade) AS max_grade
FROM courses c
JOIN enrollments e ON c.course_id = e.course_id
JOIN grades g ON e.enrollment_id = g.enrollment_id
GROUP BY c.course_id, c.title;
```
* **Explanation:** Joins class definitions to individual grade transactions, grouping entries by course to expose the minimum and maximum score markers within each class.
</details>

---

### Exercise 9: Grouping by Multiple Columns
**Task:** Count enrollments grouped by both `course_id` and `semester`. Show the course ID, semester, and enrollment count.

**Expected Columns:** `course_id`, `semester`, `enrollment_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT course_id, semester, COUNT(*) AS enrollment_count
FROM enrollments
GROUP BY course_id, semester;
```
* **Explanation:** Grouping by multiple columns creates sub-groups. In this query, rows must match *both* the `course_id` and the `semester` to belong to the same group.
</details>

---

### Exercise 10: HAVING with COUNT(DISTINCT)
**Task:** Find students who are enrolled in more than 2 different courses. Show the `student_id` and the number of distinct courses they are enrolled in.

**Expected Columns:** `student_id`, `distinct_course_count`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT student_id, COUNT(DISTINCT course_id) AS distinct_course_count
FROM enrollments
GROUP BY student_id
HAVING COUNT(DISTINCT course_id) > 2;
```
* **Explanation:** `COUNT(DISTINCT course_id)` ensures that duplicate registration entries for the exact same course are not counted twice when evaluating a student's course load.
</details>

---

## 🔴 Challenging Exercises

### Exercise 11: Department Performance Ranking
**Task:** Rank departments by their average student GPA. Show the department name, average GPA, student count, and rank. Only include departments with at least 5 students.

**Expected Columns:** `department_name`, `average_gpa`, `student_count`, `rank`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT d.name AS department_name, 
       ROUND(AVG(s.gpa), 2) AS average_gpa, 
       COUNT(s.student_id) AS student_count,
       RANK() OVER (ORDER BY AVG(s.gpa) DESC) AS rank
FROM departments d
JOIN students s ON d.department_id = s.department_id
GROUP BY d.department_id, d.name
HAVING COUNT(s.student_id) >= 5;
```
* **Explanation:** This query combines grouping, post-group filtering (`HAVING`), and a window function (`RANK()`). The window function evaluates the calculated `AVG(s.gpa)` across the filtered groups.
</details>

---

### Exercise 12: Course Enrollment with Capacity Analysis
**Task:** For each course, show the course title, enrollment count, maximum seats, remaining seats, and whether the course is "Full", "Nearly Full" (less than 5 seats left), or "Open". Only show courses that have at least 1 enrollment.

**Expected Columns:** `course_title`, `enrollment_count`, `max_seats`, `remaining_seats`, `status`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT c.title AS course_title,
       COUNT(e.student_id) AS enrollment_count,
       c.max_seats,
       (c.max_seats - COUNT(e.student_id)) AS remaining_seats,
       CASE 
           WHEN c.max_seats - COUNT(e.student_id) <= 0 THEN 'Full'
           WHEN c.max_seats - COUNT(e.student_id) < 5 THEN 'Nearly Full'
           ELSE 'Open'
       END AS status
FROM courses c
JOIN enrollments e ON c.course_id = e.course_id
GROUP BY c.course_id, c.title, c.max_seats;
```
* **Explanation:** You can reference aggregated counts (`COUNT(e.student_id)`) inside basic arithmetic evaluations and conditional `CASE WHEN` control blocks to categorize group statistics on the fly.
</details>

---

### Exercise 13: Instructor Workload and Performance
**Task:** Find instructors who teach courses where the average student grade is below 75. Show the instructor name, course count, average grade across all their courses, and number of students enrolled in their courses.

**Expected Columns:** `instructor_name`, `course_count`, `average_grade`, `total_students`

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT i.first_name || ' ' || i.last_name AS instructor_name,
       COUNT(DISTINCT c.course_id) AS course_count,
