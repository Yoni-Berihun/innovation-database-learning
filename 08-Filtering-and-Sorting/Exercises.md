
# 🧪 Exercises — Filtering and Sorting Data in PostgreSQL

These exercises help you master filtering, sorting, pattern matching, and limiting results. Practice in pgAdmin using your University Management System database.

---

## 🟢 Easy Exercises

### Exercise 8.1: Basic Filtering with WHERE

**Task:** Write SQL queries for each request.

a) Find all students with a GPA of exactly 3.5.

```sql
___________________

### b) Find all courses with exactly 4 credits.
```sql
SELECT * FROM courses WHERE credits = 4;
```

### c) Find all instructors with department_id = 10.
```sql
SELECT * FROM instructors WHERE department_id = 10;
```

### d) Find all enrollments with status = 'enrolled'.
```sql
SELECT * FROM enrollments WHERE status = 'enrolled';
```

---

### Exercise 8.2: Comparison Operators
**Task:** Write SQL queries using comparison operators.

#### a) Find all students with GPA greater than 3.0.
```sql
SELECT * FROM students WHERE gpa > 3.0;
```

#### b) Find all students with GPA less than 2.5.
```sql
SELECT * FROM students WHERE gpa < 2.5;
```

#### c) Find all courses with credits greater than or equal to 4.
```sql
SELECT * FROM courses WHERE credits >= 4;
```

#### d) Find all students who are NOT active (status != 'active').
```sql
SELECT * FROM students WHERE status != 'active';
```

---

### Exercise 8.3: Sorting Results
**Task:** Write SQL queries with ORDER BY.

#### a) List all students sorted by last name A-Z.
```sql
SELECT * FROM students ORDER BY last_name ASC;
```

#### b) List all courses sorted by credits from highest to lowest.
```sql
SELECT * FROM courses ORDER BY credits DESC;
```

#### c) List all instructors sorted by hire date, newest first.
```sql
SELECT * FROM instructors ORDER BY hire_date DESC;
```

---

### Exercise 8.4: Pattern Matching with LIKE
**Task:** Write SQL queries using LIKE.

#### a) Find all students whose first name starts with A.
```sql
SELECT * FROM students WHERE first_name LIKE 'A%';
```

#### b) Find all courses with titles containing "Programming".
```sql
SELECT * FROM courses WHERE title LIKE '%Programming%';
```

#### c) Find all students whose email ends with "@university.edu".
```sql
SELECT * FROM students WHERE email LIKE '%@university.edu';
```

---

### Exercise 8.5: Limiting Results
**Task:** Write SQL queries with LIMIT.

#### a) Show only the first 5 students alphabetically by last name.
```sql
SELECT * FROM students ORDER BY last_name ASC LIMIT 5;
```

#### b) Show the top 3 courses by credits.
```sql
SELECT * FROM courses ORDER BY credits DESC LIMIT 3;
```

#### c) Show the 10 most recently enrolled students.
```sql
SELECT * FROM students ORDER BY enrollment_date DESC LIMIT 10;
```

---

## 🟡 Medium Exercises

### Exercise 8.6: Combining Conditions with AND
**Task:** Write SQL queries combining multiple conditions.

#### a) Find all active students with GPA above 3.5.
```sql
SELECT * FROM students WHERE status = 'active' AND gpa > 3.5;
```

#### b) Find all Computer Science courses (department_id = 10) with 4 credits.
```sql
SELECT * FROM courses WHERE department_id = 10 AND credits = 4;
```

#### c) Find all students who enrolled on '2024-09-01' AND have GPA >= 3.0.
```sql
SELECT * FROM students WHERE enrollment_date = '2024-09-01' AND gpa >= 3.0;
```

---

### Exercise 8.7: Combining Conditions with OR
**Task:** Write SQL queries with OR.

#### a) Find all students in department 10 OR department 20.
```sql
SELECT * FROM students WHERE major_department_id = 10 OR major_department_id = 20;
```

#### b) Find all courses with 3 credits OR 4 credits.
```sql
SELECT * FROM courses WHERE credits = 3 OR credits = 4;
```

#### c) Find all students whose last name starts with K OR M.
```sql
SELECT * FROM students WHERE last_name LIKE 'K%' OR last_name LIKE 'M%';
```

---

### Exercise 8.8: Combining AND and OR
**Task:** Write SQL queries mixing AND and OR. Use parentheses.

#### a) Find all students with GPA above 3.5 who are in either department 10 or department 20.
```sql
SELECT * FROM students 
WHERE gpa > 3.5 AND (major_department_id = 10 OR major_department_id = 20);
```

#### b) Find all courses that are either in department 10 with 4 credits, OR in department 20 with any credits.
```sql
SELECT * FROM courses 
WHERE (department_id = 10 AND credits = 4) OR department_id = 20;
```

---

### Exercise 8.9: Full Queries with Filter, Sort, and Limit
**Task:** Write complete queries for these real-world requests.

#### a) Find the top 5 students by GPA who are active.
```sql
SELECT * FROM students 
WHERE status = 'active' 
ORDER BY gpa DESC 
LIMIT 5;
```

#### b) Find the first 10 courses alphabetically that have 3 or more credits.
```sql
SELECT * FROM courses 
WHERE credits >= 3 
ORDER BY title ASC 
LIMIT 10;
```

#### c) Find the 3 most recently hired instructors in department 10.
```sql
SELECT * FROM instructors 
WHERE department_id = 10 
ORDER BY hire_date DESC 
LIMIT 3;
```

---

### Exercise 8.10: Case-Insensitive Search
**Task:** Write case-insensitive queries using ILIKE.

#### a) Find all students whose first name starts with 'a' or 'A'.
```sql
SELECT * FROM students WHERE first_name ILIKE 'a%';
```

#### b) Find all courses with "data" anywhere in the title (case-insensitive).
```sql
SELECT * FROM courses WHERE title ILIKE '%data%';
```

#### c) Find all students whose email contains "UNIVERSITY" in any case combination.
```sql
SELECT * FROM students WHERE email ILIKE '%UNIVERSITY%';
```

---

## 🔴 Challenging Exercises

### Exercise 8.11: Academic Standing Report
**Task:** The registrar needs a report on student academic standing.

#### Part A: Find all students on academic probation (GPA < 2.0). Sort by GPA ascending. Show first name, last name, and GPA.
```sql
SELECT first_name, last_name, gpa 
FROM students 
WHERE gpa < 2.0 
ORDER BY gpa ASC;
```

#### Part B: Find all students on the Dean's List (GPA >= 3.5). Sort by GPA descending, then by last name ascending. Limit to top 20.
```sql
SELECT * FROM students 
WHERE gpa >= 3.5 
ORDER BY gpa DESC, last_name ASC 
LIMIT 20;
```

#### Part C: Find all students with GPA between 2.0 and 3.0 (inclusive). Sort by GPA descending. How many are there?
```sql
SELECT * FROM students 
WHERE gpa BETWEEN 2.0 AND 3.0 
ORDER BY gpa DESC;
-- Note: To get the exact count, a developer would use: SELECT COUNT(*) FROM students WHERE gpa BETWEEN 2.0 AND 3.0;
```

---

### Exercise 8.12: Course Catalog Search
**Task:** Build queries for a course catalog search feature.

#### Part A: A student searches for courses with "Introduction" in the title. Write the query, sorted alphabetically by title.
```sql
SELECT * FROM courses 
WHERE title ILIKE '%Introduction%' 
ORDER BY title ASC;
```

#### Part B: A student wants all 4-credit Computer Science courses (department_id = 10). Sort by course code. Limit to 5 results.
```sql
SELECT * FROM courses 
WHERE credits = 4 AND department_id = 10 
ORDER BY course_code ASC 
LIMIT 5;
```

#### Part C: An advisor needs all courses in departments 10, 20, or 30 that have 3 or more credits. Sort by department, then by credits descending.
```sql
SELECT * FROM courses 
WHERE department_id IN (10, 20, 30) AND credits >= 3 
ORDER BY department_id ASC, credits DESC;
```

---

### Exercise 8.13: Enrollment Analysis
**Task:** The enrollment office needs data for planning.

#### Part A: Find all enrollments from the last 30 days. Sort by enrollment date descending.
```sql
SELECT * FROM enrollments 
WHERE enrollment_date >= CURRENT_DATE - INTERVAL '30 days' 
ORDER BY enrollment_date DESC;
```

#### Part B: Find the 5 most popular courses by enrollment count. (Assume you can count enrollments per course_id.)
```sql
SELECT course_id, COUNT(*) AS enrollment_count 
FROM enrollments 
GROUP BY course_id 
ORDER BY enrollment_count DESC 
LIMIT 5;
```

#### Part C: Find all students whose last name starts with T, K, or M, who have GPA above 3.0, and who enrolled in 2024. Sort by last name, then first name. Limit to 15 results.
```sql
SELECT * FROM students 
WHERE (last_name LIKE 'T%' OR last_name LIKE 'K%' OR last_name LIKE 'M%')
  AND gpa > 3.0 
  AND enrollment_date BETWEEN '2024-01-01' AND '2024-12-31'
ORDER BY last_name ASC, first_name ASC 
LIMIT 15;
```

#### Part D: A professor wants to email all students in their courses whose email is NOT at university.edu. Find these students. Sort by email domain.
```sql
SELECT * FROM students 
WHERE email NOT LIKE '%@university.edu' 
ORDER BY SUBSTRING(email FROM '@(.*)$') ASC;
