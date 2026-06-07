# 🧪 Exercises — Aggregate Functions in PostgreSQL

These exercises help you master COUNT, SUM, AVG, MIN, and MAX through hands-on practice. Use your University Management System database in pgAdmin.

---

## 🟢 Easy Exercises

### Exercise 9.1: COUNT Basics

**Task:** Write SQL queries to answer each question.

#### a) How many students are in the database?
```sql
SELECT COUNT(*) FROM students;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM students;
```
* **Explanation:** `COUNT(*)` counts every single row in the `students` table, including rows with missing or NULL values.
</details>

#### b) How many courses are offered?
```sql
SELECT COUNT(*) FROM courses;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM courses;
```
* **Explanation:** This counts all rows in the `courses` table to give the total number of offered classes.
</details>

#### c) How many instructors are there?
```sql
SELECT COUNT(*) FROM instructors;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM instructors;
```
* **Explanation:** This returns the total count of teacher records inside the `instructors` table.
</details>

#### d) How many departments exist?
```sql
SELECT COUNT(*) FROM departments;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM departments;
```
* **Explanation:** This queries the `departments` table and counts all the distinct department structures saved in it.
</details>

---

### Exercise 9.2: COUNT with Conditions
**Task:** Write queries using COUNT with WHERE.

#### a) How many students have a GPA above 3.0?
```sql
SELECT COUNT(*) FROM students WHERE gpa > 3.0;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM students WHERE gpa > 3.0;
```
* **Explanation:** The `WHERE` clause filters the rows first, and then `COUNT(*)` returns the number of student rows that matched the `gpa > 3.0` condition.
</details>

#### b) How many courses have exactly 4 credits?
```sql
SELECT COUNT(*) FROM courses WHERE credits = 4;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM courses WHERE credits = 4;
```
* **Explanation:** This isolates and counts only the course records where the value in the `credits` column equals exactly 4.
</details>

#### c) How many enrollments have status 'enrolled'?
```sql
SELECT COUNT(*) FROM enrollments WHERE status = 'enrolled';
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT COUNT(*) FROM enrollments WHERE status = 'enrolled';
```
* **Explanation:** It calculates the active class registrations by filtering for the `'enrolled'` text pattern in the status column.
</details>

---

### Exercise 9.3: SUM Practice
**Task:** Write queries using SUM.

#### a) What is the total number of credits offered by all courses?
```sql
SELECT SUM(credits) FROM courses;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT SUM(credits) FROM courses;
```
* **Explanation:** `SUM(credits)` aggregates the integer values of all course rows into one single total running sum of credit hours.
</details>

#### b) What is the total budget of all departments?
```sql
SELECT SUM(budget) FROM departments;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT SUM(budget) FROM departments;
```
* **Explanation:** This adds up the financial budgets of every department registered in the university table.
</details>

#### c) What is the total budget of departments with budget greater than 300000?
```sql
SELECT SUM(budget) FROM departments WHERE budget > 300000;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT SUM(budget) FROM departments WHERE budget > 300000;
```
* **Explanation:** The database filters out departments with low resources first, and then sums the budgets of the remaining wealthy ones.
</details>

---

### Exercise 9.4: AVG Practice
**Task:** Write queries using AVG.

#### a) What is the average GPA of all students?
```sql
SELECT AVG(gpa) FROM students;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT AVG(gpa) FROM students;
```
* **Explanation:** `AVG(gpa)` computes the mean average of all grades, completely ignoring any student rows that contain a `NULL` GPA.
</details>

#### b) What is the average number of credits per course?
```sql
SELECT AVG(credits) FROM courses;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT AVG(credits) FROM courses;
```
* **Explanation:** This calculates the overall mean credit weight across all courses available in the active catalog.
</details>

#### c) What is the average department budget?
```sql
SELECT AVG(budget) FROM departments;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT AVG(budget) FROM departments;
```
* **Explanation:** This determines the average funding level by taking the total sum of budgets and dividing it by the number of departments.
</details>

---

### Exercise 9.5: MIN and MAX Practice
**Task:** Write queries using MIN and MAX.

#### a) What is the lowest and highest GPA among students?
```sql
SELECT MIN(gpa) AS lowest_gpa, MAX(gpa) AS highest_gpa FROM students;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT MIN(gpa) AS lowest_gpa, MAX(gpa) AS highest_gpa FROM students;
```
* **Explanation:** You can run multiple aggregate functions in a single statement. `MIN` finds the smallest numeric rank, and `MAX` isolates the highest.
</details>

#### b) What is the smallest and largest department budget?
```sql
SELECT MIN(budget) AS smallest_budget, MAX(budget) AS largest_budget FROM departments;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT MIN(budget) AS smallest_budget, MAX(budget) AS largest_budget FROM departments;
```
* **Explanation:** This scans the budget spectrum to identify the minimum and maximum financial allocations within the university.
</details>

#### c) What is the earliest and latest enrollment date?
```sql
SELECT MIN(enrollment_date) AS earliest_date, MAX(enrollment_date) AS latest_date FROM students;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT MIN(enrollment_date) AS earliest_date, MAX(enrollment_date) AS latest_date FROM students;
```
* **Explanation:** `MIN` on a date type field returns the oldest chronological date, while `MAX` extracts the most recent date string.
</details>

---

## 🟡 Medium Exercises

### Exercise 9.6: Combined Statistics
**Task:** Write a single query that shows all these statistics about students:
* total number of students
* average GPA
* lowest GPA
* highest GPA

```sql
SELECT 
    COUNT(*) AS total_students, 
    AVG(gpa) AS average_gpa, 
    MIN(gpa) AS lowest_gpa, 
    MAX(gpa) AS highest_gpa 
FROM students;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT 
    COUNT(*) AS total_students, 
    AVG(gpa) AS average_gpa, 
    MIN(gpa) AS lowest_gpa, 
    MAX(gpa) AS highest_gpa 
FROM students;
```
* **Explanation:** Combining all metrics into a single SELECT clause generates a highly readable, high-utility summary dashboard row from the main student table.
</details>

---

### Exercise 9.7: Department Analysis
**Task:** Write queries to analyze departments.

#### a) What is the total budget of all departments?
```sql
SELECT SUM(budget) AS total_university_budget FROM departments;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT SUM(budget) AS total_university_budget FROM departments;
```
* **Explanation:** This adds up the funding numbers across all records to display total institutional operational costs.
</details>

#### b) What is the average budget per department?
```sql
SELECT AVG(budget) AS average_department_budget FROM departments;
```

<details>
<summary><b>🔍 Show Answer</b></summary>

```sql
SELECT AVG(budget) AS average_department_budget FROM departments;
```
* **Explanation:** Uses the standard relational averaging technique to discover the average budget baseline.
</details>

#### c) How many departments have a budget above 400000?
```sql
