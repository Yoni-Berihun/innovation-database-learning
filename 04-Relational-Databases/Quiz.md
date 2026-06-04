# ✅ Quiz — Relational Databases

Test your understanding before moving to the next module. There are 10 questions. Take your time and think carefully.

---

## Question 1 (Multiple Choice)

What does the word "relational" refer to in a relational database?

- [ ] The relationships between people who use the database
- [ ] The relationships (connections) between tables of data
- [ ] The ability to relate to users emotionally
- [ ] The relationship between the database and the operating system

---

## Question 2 (Multiple Choice)

Which of the following is NOT a core component of the relational model?

- [ ] Tables
- [ ] Rows
- [ ] Columns
- [ ] Graph nodes

---

## Question 3 (Multiple Choice)

What is the purpose of a Foreign Key?

- [ ] To encrypt data in a table
- [ ] To connect one table to another by referencing a Primary Key
- [ ] To make a table sort faster
- [ ] To create a backup of the table

---

## Question 4 (Multiple Choice)

In our University Management System, which table contains Foreign Keys to BOTH the `students` and `courses` tables?

- [ ] `departments`
- [ ] `instructors`
- [ ] `enrollments`
- [ ] `grades`

---

## Question 5 (Multiple Choice)

What is referential integrity?

- [ ] The ability to reference data quickly
- [ ] The database's enforcement of valid relationships between tables
- [ ] The process of creating reference manuals
- [ ] The integrity of the database administrator

---

## Question 6 (Multiple Choice)

Why do relational databases use separate tables instead of one giant table?

- [ ] To make the database more complicated
- [ ] To reduce data duplication and improve consistency
- [ ] Because SQL only works with small tables
- [ ] To save disk space only

---

## Question 7 (Short Answer)

Explain the difference between data and relationships in a relational database. Give one example of each from the University Management System.

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 8 (Short Answer)

Why is it generally a bad idea to use a person's email address as a Primary Key? Give at least two reasons.

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 9 (Short Answer)

Look at the following tables and answer the questions:
### 🎓 University Database Tables

#### **1. TABLE: students**

| student_id (PK) | first_name | last_name | email |
| :--- | :--- | :--- | :--- |
| 1001 | Alice | Johnson | alice@uni.edu |
| 1002 | Bob | Smith | bob@uni.edu |

#### **2. TABLE: enrollments**

| enrollment_id (PK) | student_id (FK) | course_id (FK) | status |
| :--- | :--- | :--- | :--- |
| 10001 | 1001 | 501 | enrolled |
| 10002 | 1001 | 502 | enrolled |
| 10003 | 1002 | 501 | enrolled |

#### **3. TABLE: courses**

| course_id (PK) | course_code | title | instructor_id (FK) |
| :--- | :--- | :--- | :--- |
| 501 | CS101 | Intro to Programming | 1 |
| 502 | CS201 | Database Systems | 2 |

#### **4. TABLE: instructors**

| instructor_id (PK) | first_name | last_name | department_id (FK) |
| :--- | :--- | :--- | :--- |
| 1 | Sarah | Smith | 10 |
| 2 | Michael | Jones | 10 |

#### **5. TABLE: departments**

| department_id (PK) | name | building |
| :--- | :--- | :--- |
| 10 | Computer Science | Tech Building |
| 20 | Mathematics | Science Hall |

a) What is the name of the instructor who teaches the course that Bob is enrolled in?

_______________________________________________

b) Trace the complete path: List the tables you visit and the Foreign Keys you follow.

_______________________________________________

c) What would happen if you tried to delete the Computer Science department (`department_id = 10`)?

_______________________________________________

---

## Question 10 (Short Answer)

A small online store currently uses one spreadsheet to track everything. The owner wants to know why they should switch to a relational database. Write a brief explanation (3-5 sentences) using concepts from this module.

_______________________________________________
_______________________________________________
_______________________________________________
_______________________________________________
_______________________________________________

---

## ✅ Answer Key

<details>
<summary>Click to reveal answers and explanations</summary>

### Question 1
**Correct answer:** The relationships (connections) between tables of data

**Why:** "Relational" refers to how tables are connected through keys. It's not about people, emotions, or the operating system.

---

### Question 2
**Correct answer:** Graph nodes

**Why:** Tables, rows, and columns are the core components of the relational model. Graph nodes belong to graph databases, a completely different type of database system.

---

### Question 3
**Correct answer:** To connect one table to another by referencing a Primary Key

**Why:** Foreign Keys create relationships between tables. They reference Primary Keys in other tables. They don't encrypt data, sort tables, or create backups.

---

### Question 4
**Correct answer:** enrollments

**Why:** The `enrollments` table is the junction table that connects students and courses. It contains `student_id` (FK to students) and `course_id` (FK to courses).

---

### Question 5
**Correct answer:** The database's enforcement of valid relationships between tables

**Why:** Referential integrity ensures that Foreign Key values always point to existing Primary Keys. It prevents orphan records and maintains data consistency.

---

### Question 6
**Correct answer:** To reduce data duplication and improve consistency

**Why:** Separate tables eliminate redundancy (storing the same data multiple times) and ensure that when you update something, you only update it in one place. This is the core principle of relational design.

---

### Question 7
**Sample good answer:**

Data is the actual information stored — like a student's name, a course's title, or an instructor's salary. Relationships are the connections between pieces of data — like which student is enrolled in which course, or which instructor teaches which course.

**Example from University System:**
* Data: Alice's email is "alice@uni.edu" (stored in the `students` table)
* Relationship: Alice is enrolled in Intro to Programming (stored in the `enrollments` table as `student_id = 1001` and `course_id = 501`)

*(Any clear distinction with valid examples receives full credit.)*

---

### Question 8
**Sample good answer:**

Email addresses make poor Primary Keys because:
1. They can change — people switch email providers or change addresses. Changing a Primary Key breaks all Foreign Key references.
2. They might not be unique — some organizations share email addresses, or systems might allow duplicates.
3. They can be NULL — not everyone has an email, but Primary Keys cannot be NULL.
4. They contain meaningful data — Primary Keys should be meaningless identifiers.

*(Any two valid reasons receive full credit.)*

---

### Question 9

**a)** Sarah Smith

**b)** Path:
1. Start in `students` → find Bob (`student_id = 1002`)
2. Go to `enrollments` → find enrollment where `student_id = 1002` (`enrollment_id = 10003`, `course_id = 501`)
3. Go to `courses` → find course where `course_id = 501` (`instructor_id = 1`)
4. Go to `instructors` → find instructor where `instructor_id = 1` (`first_name = "Sarah"`, `last_name = "Smith"`)

**c)** The database would reject the deletion because referential integrity prevents deleting a department that still has instructors assigned to it (Dr. Smith and Dr. Jones both have `department_id = 10`).

---

### Question 10
**Sample good answer:**

A relational database would organize your data into separate, connected tables instead of one giant spreadsheet. This means customer information is stored once, not repeated on every order. If a customer changes their email, you update it in one place — not hundreds. The database also enforces rules that prevent errors, like creating an order for a customer who doesn't exist. You'll be able to answer complex questions instantly, like "Which products are most popular with repeat customers?"

*(Any answer that mentions reduced duplication, data consistency, relationships, referential integrity, or query power receives full credit.)*

</details>

> 💬 **Remember:** The relational model is the foundation of modern data management. Every SQL query you'll write, every database you'll design, and every system you'll build starts with these concepts. Understanding how tables connect through keys is a skill that will serve you throughout your entire career.

> 🚀 **Coming up next:** We'll install PostgreSQL and start writing real SQL. Every table, row, column, key, and relationship you've learned about will become actual code that you can run, test, and see in action. Get ready — this is where it gets exciting!
