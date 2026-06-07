# 🎓 Projects & Resources

&gt; **Welcome to the Projects & Resources section!**
&gt;
&gt; You have completed Modules 1–18. Now it is time to apply everything you learned to real-world scenarios.
&gt;
&gt; These projects use the **University Management System** schema you have been working with throughout the course. Each project is designed to be beginner-friendly but realistic — the kind of work you might do in an internship or junior developer role.
&gt;
&gt; **Tip:** Don't rush. Complete the tasks in order. If you get stuck, review the relevant module or check the Learning Resources section at the bottom of this page.

---

## 📋 Table of Contents
1. [Project 1: Student Transcript Generator](#project-1-student-transcript-generator)
2. [Project 2: Course Enrollment Analytics Dashboard](#project-2-course-enrollment-analytics-dashboard)
3. [Project 3: Department Performance Report](#project-3-department-performance-report)


---

# Part 1: Real-World Projects

---

## Project 1: Student Transcript Generator

### 📖 Description
The university registrar needs a system to generate official student transcripts. A transcript must show all courses a student has taken, the grades received, credits earned, and the student's cumulative GPA. The system must handle missing grades gracefully (e.g., courses still in progress).

### 🎯 Learning Objectives
- Join multiple tables (`students`, `enrollments`, `courses`, `grades`)
- Calculate cumulative GPA using aggregations
- Handle NULL values (missing grades) with `COALESCE`
- Format output cleanly using column aliases
- Use a CTE to organize the query

### 📊 Required Tables
- `students`
- `enrollments`
- `courses`
- `grades`

### ✅ Step-by-Step Tasks

#### Task 1: Basic Join
Write a query that joins `students`, `enrollments`, `courses`, and `grades` to show:
- `student_name`
- `course_title`
- `credits`
- `letter_grade`
- `numeric_grade`
- `semester`

&gt; **Hint:** Use `LEFT JOIN` for `grades` because some enrollments may not have grades yet.

#### Task 2: Handle Missing Grades
Modify your query to replace NULL grades with `'In Progress'` and NULL numeric grades with `0`.

&gt; **Hint:** Use `COALESCE(letter_grade, 'In Progress')` and `COALESCE(numeric_grade, 0)`.

#### Task 3: Calculate Credits Earned
Add a column that shows credits only if the student passed (numeric grade &gt;= 60). Otherwise, show `0`.

&gt; **Hint:** Use a `CASE` statement.

#### Task 4: Calculate Cumulative GPA
Write a query that calculates the student's cumulative GPA on a 4.0 scale.

&gt; **Hint:** GPA formula: `SUM(numeric_grade * credits) / SUM(credits) / 25.0`

#### Task 5: Wrap in a CTE
Organize your query using a CTE named `transcript_data` that contains the joined data, then calculate GPA in the main query.

#### Task 6: Create a View
Create a view named `student_transcript` that accepts any student and returns their transcript data.

&gt; **Hint:** You cannot parameterize a view directly. Instead, create the view with all data and filter when querying it.

#### Task 7: Generate a Formatted Report
Write a final query that produces output like:



> **Hint:** Use string concatenation (`||`) and `RPAD`/`LPAD` for formatting.

### 🏁 Expected Outcome
A complete, formatted transcript query that can be run for any student. The query should handle missing grades, calculate GPA correctly, and be wrapped in a reusable view.

### 🚀 Extension Ideas
- Add a column for "Academic Standing" (Good Standing, Probation, Dean's List) based on GPA.
- Create a function that takes a `student_id` and returns the transcript as formatted text.
- Add a trigger that logs every time a transcript is generated for audit purposes.

---

## Project 2: Course Enrollment Analytics Dashboard

### 📖 Description
The university scheduling office needs a dashboard to monitor course enrollment trends. They need to identify popular courses, courses at risk of being under-enrolled, and instructors with heavy workloads. Your job is to write the SQL queries that feed this dashboard.

### 🎯 Learning Objectives
- Use aggregations and `GROUP BY` for summary statistics
- Use CTEs to break complex queries into readable steps
- Use window functions (`RANK()`, `ROW_NUMBER()`) for rankings
- Create indexes to optimize dashboard query performance
- Build a view that combines all dashboard metrics

### 📊 Required Tables
- `courses`
- `enrollments`
- `students`
- `instructors`
- `departments`

### ✅ Step-by-Step Tasks

#### Task 1: Enrollment Counts per Course
Write a query that shows each course's title, department, and total enrollment count.

#### Task 2: Most Popular Courses
Rank courses by enrollment count. Show the top 10 most popular courses.

> **Hint:** Use `RANK() OVER (ORDER BY enrollment_count DESC)`.

#### Task 3: Course Capacity vs. Enrollment
Assume `courses` has a `max_seats` column. Write a query showing:
- `course_title`
- `max_seats`
- `enrolled_count`
- `seats_remaining`
- `fill_percentage`

> **Hint:** `fill_percentage = (enrolled_count * 100.0 / max_seats)`

#### Task 4: Under-Enrolled Courses
Identify courses with fewer than 50% seats filled. These may be cancelled.

#### Task 5: Instructor Workload
Show each instructor's name, department, and number of courses they teach. Rank instructors by workload.

> **Hint:** Use a CTE for course counts, then join to instructors.

#### Task 6: Optimize with Indexes
Identify which columns should be indexed to speed up the dashboard queries. Write `CREATE INDEX` statements.

> **Hint:** Index columns used in `WHERE`, `JOIN`, and `GROUP BY`.

#### Task 7: Create a Dashboard View
Create a view named `enrollment_dashboard` that combines all the metrics above into a single queryable source.

### 🏁 Expected Outcome
A set of optimized queries and a view that provide real-time enrollment analytics. The scheduling office can use these to make data-driven decisions about course offerings.

### 🚀 Extension Ideas
- Add a trend analysis: compare current semester enrollment to last semester.
- Create a materialized view that refreshes nightly for faster dashboard loading.
- Write a stored procedure that emails department heads when their courses are under-enrolled.

---

## Project 3: Department Performance Report

### 📖 Description
The university administration needs an annual report comparing department performance. They want to know which departments have the highest-achieving students, which are growing or shrinking, and whether faculty resources match student demand.

### 🎯 Learning Objectives
- Write complex multi-table joins
- Use subqueries and CTEs for calculated metrics
- Create and use views for reusable report sections
- Use transactions to safely update derived data
- Apply constraints and data integrity checks

### 📊 Required Tables
- `departments`
- `students`
- `instructors`
- `courses`
- `enrollments`
- `grades`

### ✅ Step-by-Step Tasks

#### Task 1: Department Student Counts
Write a query showing each department's name and current student count.

#### Task 2: Average GPA by Department
Calculate the average GPA of students in each department. Rank departments by academic performance.

> **Hint:** Use a CTE for student GPAs, then aggregate by department.

#### Task 3: Faculty-to-Student Ratio
Calculate the ratio of instructors to students for each department.

> **Hint:** `faculty_to_student_ratio = instructor_count / student_count`

#### Task 4: Course Success Rate
For each department, calculate the percentage of course enrollments that resulted in a passing grade (>= 60).

> **Hint:** This requires joining `departments` → `instructors` → `courses` → `enrollments` → `grades`.

#### Task 5: Create a Department Performance View
Create a view named `department_performance` that combines all metrics from Tasks 1–4.

#### Task 6: Safe Data Update with Transaction
The administration decides to flag underperforming departments (average GPA < 2.5). Write a transaction that:
1. Creates an `underperforming_departments` table if it doesn't exist
2. Inserts departments with average GPA < 2.5
3. Rolls back if any error occurs

#### Task 7: Data Integrity Check
Write a query that finds any data integrity issues (e.g., students with no department, instructors teaching courses in other departments, grades without enrollments).

### 🏁 Expected Outcome
A comprehensive department performance report with multiple metrics, a reusable view, and safe data update procedures. The administration can use this for funding and staffing decisions.

### 🚀 Extension Ideas
- Add year-over-year growth rates for each department.
- Create a trigger that automatically updates department statistics when a student changes departments.
- Design a normalized vs. denormalized schema debate: which is better for this report?

---


## 🗃️ Sample University Dataset

Below is a complete INSERT script for the University Management System. Use this to populate your database for the projects.

```sql
-- Departments
INSERT INTO departments (department_id, name, building, student_count) VALUES
(1, 'Computer Science', 'Engineering Hall', 0),
(2, 'Mathematics', 'Science Building', 0),
(3, 'Physics', 'Science Building', 0),
(4, 'English', 'Humanities Center', 0);

-- Students
INSERT INTO students (student_id, first_name, last_name, email, gpa, department_id, enrollment_date, status, total_credits) VALUES
(1, 'Alice', 'Johnson', 'alice@uni.edu', 3.85, 1, '2023-08-15', 'Active', 45),
(2, 'Bob', 'Smith', 'bob@uni.edu', 3.20, 1, '2023-08-15', 'Active', 42),
(3, 'Charlie', 'Brown', 'charlie@uni.edu', 2.90, 2, '2023-08-15', 'Active', 38),
(4, 'Diana', 'Prince', 'diana@uni.edu', 3.95, 1, '2022-08-15', 'Active', 60),
(5, 'Eve', 'Davis', 'eve@uni.edu', 3.50, 2, '2023-08-15', 'Active', 40),
(6, 'Frank', 'Miller', 'frank@uni.edu', 2.50, 3, '2023-08-15', 'Probation', 30),
(7, 'Grace', 'Lee', 'grace@uni.edu', 3.75, 1, '2024-08-15', 'Active', 15),
(8, 'Henry', 'Wilson', 'henry@uni.edu', 3.10, 4, '2023-08-15', 'Active', 35);

-- Instructors
INSERT INTO instructors (instructor_id, first_name, last_name, email, department_id) VALUES
(10, 'Dr. Alan', 'Turing', 'turing@uni.edu', 1),
(11, 'Dr. Grace', 'Hopper', 'hopper@uni.edu', 1),
(12, 'Dr. Katherine', 'Johnson', 'johnson@uni.edu', 2),
(13, 'Dr. Richard', 'Feynman', 'feynman@uni.edu', 3),
(14, 'Dr. Jane', 'Austen', 'austen@uni.edu', 4);

-- Courses
INSERT INTO courses (course_id, title, credits, instructor_id, max_seats, seats_available, department_id) VALUES
(101, 'Introduction to Computer Science', 3, 10, 50, 45, 1),
(102, 'Database Systems', 4, 11, 40, 35, 1),
(103, 'Web Development', 3, 11, 45, 40, 1),
(104, 'Calculus I', 4, 12, 60, 55, 2),
(105, 'Linear Algebra', 3, 12, 50, 48, 2),
(106, 'Physics I', 4, 13, 40, 38, 3),
(107, 'Creative Writing', 3, 14, 30, 28, 4);

-- Enrollments
INSERT INTO enrollments (enrollment_id, student_id, course_id, semester, status) VALUES
(500, 1, 101, 'Fall 2025', 'Active'),
(501, 1, 102, 'Fall 2025', 'Active'),
(502, 1, 103, 'Fall 2025', 'Active'),
(503, 2, 101, 'Fall 2025', 'Active'),
(504, 2, 104, 'Fall 2025', 'Active'),
(505, 3, 104, 'Fall 2025', 'Active'),
(506, 3, 105, 'Fall 2025', 'Active'),
(507, 4, 102, 'Fall 2025', 'Active'),
(508, 4, 103, 'Fall 2025', 'Active'),
(509, 5, 105, 'Fall 2025', 'Active'),
(510, 6, 106, 'Fall 2025', 'Active'),
(511, 7, 101, 'Fall 2025', 'Active'),
(512, 8, 107, 'Fall 2025', 'Active');

-- Grades
INSERT INTO grades (grade_id, enrollment_id, letter_grade, numeric_grade) VALUES
(900, 500, 'A', 95),
(901, 501, 'A-', 92),
(902, 502, 'B+', 88),
(903, 503, 'B', 85),
(904, 504, 'C+', 78),
(905, 505, 'B-', 82),
(906, 506, 'A', 96),
(907, 507, 'A', 94),
(908, 508, 'B+', 89),
(909, 509, 'B', 84),
(910, 510, 'D', 62),
(911, 511, 'A-', 91),
(912, 512, 'B+', 88);
