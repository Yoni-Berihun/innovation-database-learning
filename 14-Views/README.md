# 👁️ Views in PostgreSQL

 **Prerequisite:** Modules 01–13  


## 🎯 Learning Objectives

By the end of this module, you will be able to:

1. Explain what a view is and how it differs from a table.
2. Create, query, and drop views using `CREATE VIEW`, `SELECT`, and `DROP VIEW`.
3. Build simple views from a single table with basic filtering.
4. Build complex views using joins, aggregations, and subqueries.
5. Understand when a view is updatable and when it is not.
6. Describe the purpose of materialized views (advanced awareness).
7. Apply views to real-world university management scenarios.
8. Understand how views improve security, simplicity, and reusability.

---

## 🧠 Why Views Matter

In a real-world database, tables store raw data. But users—regardless of whether they are professors, students, or administrators—rarely need to see raw data. They need **answers**.

- A professor needs a class roster, not the entire `enrollments` table.
- A registrar needs a dean's list, not every grade ever recorded.
- A department head needs a summary of faculty workload, not raw instructor records.

**Views are saved queries that act like virtual tables.** You write the query once, save it as a view, and then query the view as if it were a real table.

### Without Views
Every time someone needs a class roster, they write a 5-table JOIN query from scratch. This is:
- Repetitive
- Error-prone
- Hard to maintain

### With Views
You write the query once, save it as `class_roster`, and then simply run:
```sql
SELECT * FROM class_roster WHERE course_title = 'Database Systems';

# 📖 What Is a View?

A view is a virtual table based on the result of a SQL query. It does not store data itself. Instead, it stores the query definition. Every time you query the view, PostgreSQL runs the underlying query behind the scenes.

### Beginner-Friendly Analogy
* **Think of a view as a custom camera lens filter:** The table is the real world (all the data). The view is a filter on your camera lens that only shows what you want: blue skies, no crowds, sharp focus. You don't change the real world; you just look at it through a specific lens.
* **Another analogy:** A view is like a saved report in Excel. The data lives in the raw sheets, but the report shows only the summarized, filtered results.

### Text Diagram
```text
┌─────────────────────────────────────┐
│           Real Tables               │
│  ┌─────────┐  ┌─────────────┐      │
│  │ students│  │ enrollments │      │
│  └─────────┘  └─────────────┘      │
│  ┌─────────┐  ┌─────────────┐      │
│  │ courses │  │   grades    │      │
│  └─────────┘  └─────────────┘      │
└─────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────┐
│              VIEW                   │
│    ┌─────────────────────────┐      │
│    │  SELECT s.first_name,   │      │
│    │         c.title,        │      │
│    │         g.letter_grade  │      │
│    │  FROM students s        │      │
│    │  JOIN enrollments e ... │      │
│    │  JOIN courses c ...     │      │
│    │  JOIN grades g ...      │      │
│    └─────────────────────────┘      │
│              │                      │
│              ▼                      │
│    ┌─────────────────────────┐      │
│    │   student_transcript    │      │
│    │   (virtual table)       │      │
│    │  ┌──────┬───────┬──────┐│      │
│    │  │ name │ course│ grade││      │
│    │  └──────┴───────┴──────┘│      │
│    └─────────────────────────┘      │
└─────────────────────────────────────┘
```
*The view is a window into the real tables. It does not copy data. It shows a live snapshot every time you look through it.*

---

## 📖 Difference Between a View and a Table





| Feature | Table | View |
| :--- | :--- | :--- |
| **Stores data?** | Yes | No (stores a query) |
| **Takes up disk space?** | Yes (for data) | Minimal (only query definition) |
| **Data changes automatically?** | N/A | Yes, when underlying table changes |
| **Can be queried with SELECT?** | Yes | Yes |
| **Can be used in JOINs?** | Yes | Yes |
| **Can be updated directly?** | Yes | Sometimes (updatable views) |
| **Primary Purpose** | Store raw data | Simplify access, secure data, reuse queries |

---

## 📖 Creating a View

### Syntax
```sql
CREATE VIEW view_name AS
SELECT columns
FROM tables
WHERE conditions;
```

### Example: Simple View — Active Students
Show all students who have at least one enrollment.
```sql
CREATE VIEW active_students AS
SELECT s.student_id, s.first_name, s.last_name, s.email
FROM students s
WHERE s.student_id IN (
    SELECT student_id FROM enrollments
);
```

#### Usage:
```sql
SELECT * FROM active_students;
```

#### Result:




| student_id | first_name | last_name | email |
| :--- | :--- | :--- | :--- |
| 1 | Alice | Johnson | alice@uni.edu |
| 2 | Bob | Smith | bob@uni.edu |
| 3 | Charlie | Brown | charlie@uni.edu |

---

## 📖 Dropping a View

### Syntax
```sql
DROP VIEW view_name;
```

### Safe version (avoids error if view doesn't exist):
```sql
DROP VIEW IF EXISTS view_name;
```

---

## 📖 Complex Views

Views become powerful when they encapsulate complex joins and calculations.

### Example: Course Enrollment Counts
Show each course title and the number of students enrolled.
```sql
CREATE VIEW course_enrollment_counts AS
SELECT
    c.course_id,
    c.title AS course_title,
    COUNT(e.student_id) AS student_count
FROM courses c
LEFT JOIN enrollments e ON c.course_id = e.course_id
GROUP BY c.course_id, c.title;
```

#### Usage:
```sql
SELECT * FROM course_enrollment_counts;
```

#### Result:




| course_id | course_title | student_count |
| :--- | :--- | :--- |
| 101 | Intro to CS | 2 |
| 102 | Database Systems | 1 |
| 103 | Web Development | 1 |

### Example: Faculty Course Load
Show each instructor and how many courses they teach.
```sql
CREATE VIEW faculty_course_load AS
SELECT
    i.instructor_id,
    i.first_name || ' ' || i.last_name AS instructor_name,
    COUNT(c.course_id) AS course_count
FROM instructors i
LEFT JOIN courses c ON i.instructor_id = c.instructor_id
GROUP BY i.instructor_id, i.first_name, i.last_name;
```

#### Usage:
```sql
SELECT * FROM faculty_course_load WHERE course_count > 1;
```

### Example: Student Transcript
Show a complete academic transcript for every student.
```sql
CREATE VIEW student_transcript AS
SELECT
    s.student_id,
    s.first_name || ' ' || s.last_name AS student_name,
    c.title AS course_title,
    g.letter_grade,
    g.numeric_grade,
    e.semester
FROM students s
JOIN enrollments e ON s.student_id = e.student_id
JOIN courses c ON e.course_id = c.course_id
LEFT JOIN grades g ON e.enrollment_id = g.enrollment_id;
```

#### Usage:
```sql
SELECT * FROM student_transcript WHERE student_name = 'Alice Johnson';
```

---

## 📖 Updatable Views

Some views can be used to modify data (`INSERT`, `UPDATE`, `DELETE`) just like a real table. These are called updatable views.

### When Is a View Updatable?
A view is generally updatable if:
1. It references only one table.
2. It does not use `GROUP BY`, `DISTINCT`, `UNION`, or aggregate functions (`COUNT`, `SUM`, etc.).
3. It does not use `JOIN` (in most cases).

### Example: Updatable View
```sql
CREATE VIEW cs_students AS
SELECT student_id, first_name, last_name, email, gpa
FROM students
WHERE department_id = 1;  -- Computer Science department
```

You can update through this view:
```sql
UPDATE cs_students
SET gpa = 3.8
WHERE student_id = 1;
-- ✅ This updates the underlying students table.
```

But you cannot insert a row that violates the view's filter easily:
```sql
INSERT INTO cs_students (first_name, last_name, email, gpa)
VALUES ('Eve', 'Davis', 'eve@uni.edu', 3.5);
-- ⚠️ This may fail if department_id is NOT NULL and not provided,
-- or if it defaults to a different department.
```

### Non-Updatable View Example
```sql
CREATE VIEW course_enrollment_counts AS
SELECT c.title, COUNT(e.student_id) AS student_count
FROM courses c
LEFT JOIN enrollments e ON c.course_id = e.course_id
GROUP BY c.title;
```

```sql
UPDATE course_enrollment_counts SET student_count = 50;
-- ❌ ERROR! Views with GROUP BY and aggregate functions are not updatable.
```

💡 **Rule of thumb:** If your view looks like a single table with simple rows, it might be updatable. If it summarizes or joins multiple tables, it is read-only.

---

## 📖 Materialized Views (Advanced Awareness)

A materialized view is like a regular view, but it stores the query result on disk as a physical table. It does not update automatically when the underlying data changes.

### Syntax
```sql
CREATE MATERIALIZED VIEW student_gpa_summary AS
SELECT department_id, AVG(gpa) AS avg_gpa
FROM students
GROUP BY department_id;
```

### Refresh the data manually:
```sql
REFRESH MATERIALIZED VIEW student_gpa_summary;
```

### When to Use
* The query is very slow (complex aggregations on millions of rows).
* The data does not need to be real-time.
* You run the same report every morning and want instant results.

> ⚠️ **Beginner Note:** Materialized views are advanced. For now, just know they exist. You will use regular views for most tasks.

---

## 🌍 Real-World Applications

### 1. Hiding Sensitive Columns
A university help desk should see student names and emails, but not their GPA or financial data.
```sql
CREATE VIEW student_directory AS
SELECT student_id, first_name, last_name, email
FROM students;
```
*The help desk gets access to `student_directory`, not the full `students` table.*

### 2. Simplifying Complex Reports
A department head runs a weekly report. Instead of writing a 6-table `JOIN` every Monday, they query a view:
```sql
SELECT * FROM faculty_course_load WHERE course_count = 0;
-- Find instructors with no courses (need more assignments).
```

### 3. Providing a Stable Interface
If the database designer renames a column from `last_name` to `surname`, all applications using `SELECT last_name` would break. But if they use a view:
```sql
CREATE VIEW students_legacy AS
SELECT student_id, first_name, surname AS last_name, email
FROM students;
```
*Applications querying `students_legacy` continue working without changes.*

### 4. Granting Access Without Direct Table Permissions
Database administrators can grant `SELECT` permission on a view without giving access to the underlying tables. This is a core security practice.

---

## ⚠️ Common Beginner Mistakes





| Mistake | Example | Fix |
| :--- | :--- | :--- |
