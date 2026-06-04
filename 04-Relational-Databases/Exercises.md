# 🧪 Exercises — Relational Databases

Welcome to your exercises! You've learned what makes a database relational — tables, keys, relationships, and referential integrity. Now it's time to practice seeing relational structures, tracing connections, and spotting good and bad design.

No computer needed. Just your brain, a pencil, and some paper.

---

## 🟢 Easy Exercises

These exercises reinforce the core concepts of relational databases.

---

### Exercise 4.1: Identify the Components

**Task:** Look at the table below and identify its components.

TABLE: courses


| course_id (PK) | course_code | title                | credits | department_id (FK) |
|----------------|-------------|----------------------|---------|--------------------|
| 501            | CS101       | Intro to Programming | 4       | 10                 |
| 502            | CS201       | Database Systems     | 3       | 10                 |
| 503            | MATH101     | Calculus I           | 4       | 20                 |
| 504            | CS301       | Advanced Algorithms  | 3       | 10                 |


**Questions:**

a) How many rows does this table have? ___________________

b) How many columns does this table have? ___________________

c) What is the Primary Key? ___________________

d) What is the Foreign Key, and which table does it reference? ___________________

e) What does `department_id = 10` mean in the first row? ___________________

---

### Exercise 4.2: Primary Key or Foreign Key?

**Task:** For each column below, identify whether it should be a Primary Key, a Foreign Key, or a regular attribute.

| Column | Table | PK / FK / Regular? | Why? |
|--------|-------|-------------------|------|
| student_id | students | | |
| email | students | | |
| department_id | departments | | |
| department_id | instructors | | |
| course_id | courses | | |
| course_id | enrollments | | |
| enrollment_date | enrollments | | |
| score | grades | | |
| instructor_id | courses | | |

---

### Exercise 4.3: Trace the Relationship

**Task:** Using the University Management System tables, trace how you would find the answer to each question.

**Example:** What is Dr. Sarah Smith's salary?

Step 1: Look in the `instructors` table for `first_name = "Sarah"` and `last_name = "Smith"`
Step 2: Find the `salary` column in that row
Answer: $85,000.00

**Now you try:**

a) What building is the Mathematics department in?

Step 1: _______________________________________________
Step 2: _______________________________________________
Answer: _______________________________________________

b) Which instructor teaches Database Systems?

Step 1: _______________________________________________
Step 2: _______________________________________________
Step 3: _______________________________________________
Answer: _______________________________________________

c) What is Carol Williams' GPA?

Step 1: _______________________________________________
Step 2: _______________________________________________
Answer: _______________________________________________

d) How many credit hours is the course Alice got 85% on?

Step 1: _______________________________________________
Step 2: _______________________________________________
Step 3: _______________________________________________
Step 4: _______________________________________________
Answer: _______________________________________________

---

### Exercise 4.4: Spot the Relational Principles

**Task:** Read each statement and decide if it follows good relational database design.

| Statement | Good Design? (Yes/No) | Why? |
|-----------|----------------------|------|
| Every table has a Primary Key | | |
| A table stores students and their courses in the same row | | |
| Foreign Keys reference Primary Keys in other tables | | |
| Student email addresses are used as Primary Keys | | |
| The database prevents deleting a department that still has instructors | | |
| A student's total GPA is stored in the students table and updated manually | | |
| Enrollment records connect students to courses | | |
| One giant table stores all university data | | |

---

### Exercise 4.5: Match the Concept

**Task:** Match each concept on the left with its description on the right.

| Concept | Description |
|---------|-------------|
| Table | a) Ensures relationships between tables stay valid |
| Row | b) A column that references a Primary Key in another table |
| Column | c) One record in a table |
| Primary Key | d) A collection of related data organized into rows and columns |
| Foreign Key | e) Uniquely identifies each row in a table |
| Referential Integrity | f) Defines one attribute of the data |

---

## 🟡 Medium Exercises

These exercises ask you to apply relational concepts to design and analysis problems.

---

### Exercise 4.6: Design a Relational Structure

**Task:** Design a relational database for a **music streaming app** (like Spotify).

**Entities to include:**
* User (listener)
* Artist
* Album
* Song
* Playlist

**Requirements:**

a) List all tables with their Primary Keys

| Table | Primary Key |
|-------|-------------|
| | |
| | |
| | |
| | |
| | |

b) Identify relationships between tables

| Table A | Relationship | Table B | Type (1:1 / 1:N / M:N) |
|---------|-------------|---------|------------------------|
| | | | |
| | | | |
| | | | |
| | | | |

c) Identify Foreign Keys

| Foreign Key | In Which Table? | References Which Table? |
|-------------|----------------|------------------------|
| | | |
| | | |
| | | |
| | | |

d) Which relationships are Many-to-Many? How will you resolve them?

_______________________________________________
_______________________________________________

---

### Exercise 4.7: Fix the Broken Design

**Task:** Below is a poorly designed table for an online store. Identify at least **5 problems** and explain how to fix each one using relational principles.


| customer_name | customer_email | customer_phone | product_1 | product_2 | product_3 | total |
|---------------|----------------|----------------|-----------|-----------|-----------|-------|
| Alice Johnson | alice@mail.com | 555-0100       | Laptop    | Mouse     | *(null)*  | 1200  |
| Bob Smith     | bob@mail.com   | 555-0101       | Phone     | *(null)*  | *(null)*  | 800   |
| Alice Johnson | alice@mail.com | 555-0100       | Tablet    | Keyboard  | Monitor   | 1500  |

**Problems:**

1. _______________________________________________
   **Fix:** _______________________________________________

2. _______________________________________________
   **Fix:** _______________________________________________

3. _______________________________________________
   **Fix:** _______________________________________________

4. _______________________________________________
   **Fix:** _______________________________________________

5. _______________________________________________
   **Fix:** _______________________________________________

---

### Exercise 4.8: Referential Integrity Scenarios

**Task:** For each scenario, predict what a relational database with referential integrity would do.

**Scenario A:** You try to insert an enrollment record with `student_id = 9999`, but no student with that ID exists.

What happens? _______________________________________________
Why? _______________________________________________

**Scenario B:** You try to delete a course that has 30 students enrolled in it.

What happens? _______________________________________________
Why? _______________________________________________

**Scenario C:** You update a student's `student_id` from `1001` to `1001` (same value).

What happens? _______________________________________________
Why? _______________________________________________

**Scenario D:** You try to change an instructor's `department_id` from `10` to `99`, but department `99` doesn't exist.

What happens? _______________________________________________
Why? _______________________________________________

**Scenario E:** You delete a student who has no enrollments and no grades.

What happens? _______________________________________________
Why? _______________________________________________

---

### Exercise 4.9: The Junction Table Deep Dive

**Task:** In our University Management System, `enrollments` is a junction table that resolves the Many-to-Many relationship between `students` and `courses`.

**Part A:** Answer these questions about the `enrollments` table.

1. What is its Primary Key? ___________________
2. What Foreign Keys does it contain? ___________________
3. What additional information does it store beyond the connections? ___________________
4. Why can't we just put `student_id` in the `courses` table? ___________________
5. Why can't we just put `course_id` in the `students` table? ___________________

**Part B:** Imagine a student can enroll in the same course multiple times (for example, retaking a failed course). How would you modify the design to handle this?

_______________________________________________
_______________________________________________

---

### Exercise 4.10: Compare Approaches

**Task:** A small business wants to track customer orders. They have two design options.

**Option A: Single Table**
order_data

| order_id | customer_name | customer_email | product_name | quantity | price | order_date |
|----------|---------------|----------------|--------------|----------|-------|------------|
| 1001     | Alice Johnson | alice@mail.com | Laptop       | 1        | 1100  | 2026-06-01 |
| 1001     | Alice Johnson | alice@mail.com | Mouse        | 1        | 100   | 2026-06-01 |
| 1002     | Bob Smith     | bob@mail.com   | Phone        | 1        | 800   | 2026-06-02 |
| 1003     | Alice Johnson | alice@mail.com | Tablet       | 1        | 500   | 2026-06-04 |

**Questions:**

a) If a customer changes their email, which option is easier to update? Why?

_______________________________________________

b) If the business starts selling 100 products instead of 10, which option scales better? Why?

_______________________________________________

c) Which option prevents you from creating an order for a non-existent customer? How?

_______________________________________________

d) Which option allows a single order to contain multiple products? How?

_______________________________________________

e) Which option follows relational database principles? List three specific principles it demonstrates.

1. _______________________________________________
2. _______________________________________________
3. _______________________________________________

---

## 🔴 Challenging Exercises

These exercises present realistic scenarios that require deep understanding of relational design.

---

### Exercise 4.11: Design a Hospital Relational Database

**Task:** Design a complete relational database for a hospital.

**Requirements:**

**Entities to track:**
* Patient
* Doctor
* Department
* Appointment
* MedicalRecord
* Prescription
* Medication
* Nurse
* Room

**Part A: List all tables with Primary Keys**

| Table | Primary Key | Purpose |
|-------|-------------|---------|
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |

**Part B: Define all relationships**

| Table A | Table B | Relationship Type | Foreign Key Location |
|---------|---------|-------------------|---------------------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Part C: Identify Many-to-Many relationships and design junction tables**

_______________________________________________
_______________________________________________

**Part D: Design questions**

1. A patient can have many appointments, and a doctor can have many appointments. What type of relationship is this, and where does the Foreign Key go?

_______________________________________________

2. A prescription can include multiple medications, and a medication can appear in many prescriptions. How do you model this?

_______________________________________________

3. If a doctor leaves the hospital, what should happen to their appointments? Consider at least two options and their trade-offs.

_______________________________________________
_______________________________________________

4. How would you ensure that a room isn't double-booked for two patients at the same time?

_______________________________________________

---

### Exercise 4.12: The University System — Relational Design Validation

**Task:** Validate the University Management System relational design by answering these advanced questions.

**Part A: Data redundancy check**

For each piece of information below, identify exactly ONE table where it should be stored.

| Information | Table | Column |
|-------------|-------|--------|
| Student's email | | |
| Department's budget | | |
| Course's credit hours | | |
| Instructor's hire date | | |
| Grade score | | |
| Enrollment status | | |

**Part B: Trace complex queries**

Trace the complete path for each question:

a) "What is the total budget of the department that offers the course 'Database Systems'?"

Tables visited: _______________________________________________
Foreign Keys followed: _______________________________________________

b) "List all students who are taught by Dr. Sarah Smith."

Tables visited: _______________________________________________
Foreign Keys followed: _______________________________________________

c) "What is the average grade for Assignment 1 in Intro to Programming?"

Tables visited: _______________________________________________
Foreign Keys followed: _______________________________________________

**Part C: Design challenge**

The university wants to add a new feature: students can rate courses after completing them.

1. What new table(s) do you need?

_______________________________________________

2. What relationships does this create?

_______________________________________________

3. How do you ensure a student can only rate a course they've completed?

_______________________________________________

---

### Exercise 4.13: Migration Scenario

**Task:** A company currently stores all data in spreadsheets and wants to move to a relational database.

**Current spreadsheet structure:**


**Questions:**

a) If a customer changes their email, which option is easier to update? Why?

_______________________________________________

b) If the business starts selling 100 products instead of 10, which option scales better? Why?

_______________________________________________

c) Which option prevents you from creating an order for a non-existent customer? How?

_______________________________________________

d) Which option allows a single order to contain multiple products? How?

_______________________________________________

e) Which option follows relational database principles? List three specific principles it demonstrates.

1. _______________________________________________
2. _______________________________________________
3. _______________________________________________

---

## 🔴 Challenging Exercises

These exercises present realistic scenarios that require deep understanding of relational design.

---

### Exercise 4.11: Design a Hospital Relational Database

**Task:** Design a complete relational database for a hospital.

**Requirements:**

**Entities to track:**
* Patient
* Doctor
* Department
* Appointment
* MedicalRecord
* Prescription
* Medication
* Nurse
* Room

**Part A: List all tables with Primary Keys**

| Table | Primary Key | Purpose |
|-------|-------------|---------|
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |

**Part B: Define all relationships**

| Table A | Table B | Relationship Type | Foreign Key Location |
|---------|---------|-------------------|---------------------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Part C: Identify Many-to-Many relationships and design junction tables**

_______________________________________________
_______________________________________________

**Part D: Design questions**

1. A patient can have many appointments, and a doctor can have many appointments. What type of relationship is this, and where does the Foreign Key go?

_______________________________________________

2. A prescription can include multiple medications, and a medication can appear in many prescriptions. How do you model this?

_______________________________________________

3. If a doctor leaves the hospital, what should happen to their appointments? Consider at least two options and their trade-offs.

_______________________________________________
_______________________________________________

4. How would you ensure that a room isn't double-booked for two patients at the same time?

_______________________________________________

---

### Exercise 4.12: The University System — Relational Design Validation

**Task:** Validate the University Management System relational design by answering these advanced questions.

**Part A: Data redundancy check**

For each piece of information below, identify exactly ONE table where it should be stored.

| Information | Table | Column |
|-------------|-------|--------|
| Student's email | | |
| Department's budget | | |
| Course's credit hours | | |
| Instructor's hire date | | |
| Grade score | | |
| Enrollment status | | |

**Part B: Trace complex queries**

Trace the complete path for each question:

a) "What is the total budget of the department that offers the course 'Database Systems'?"

Tables visited: _______________________________________________
Foreign Keys followed: _______________________________________________

b) "List all students who are taught by Dr. Sarah Smith."

Tables visited: _______________________________________________
Foreign Keys followed: _______________________________________________

c) "What is the average grade for Assignment 1 in Intro to Programming?"

Tables visited: _______________________________________________
Foreign Keys followed: _______________________________________________

**Part C: Design challenge**

The university wants to add a new feature: students can rate courses after completing them.

1. What new table(s) do you need?

_______________________________________________

2. What relationships does this create?

_______________________________________________

3. How do you ensure a student can only rate a course they've completed?

_______________________________________________

---

### Exercise 4.13: Migration Scenario

**Task:** A company currently stores all data in spreadsheets and wants to move to a relational database.

**Current spreadsheet structure:**

| date | salesperson_name | region | product_name | units_sold | revenue | customer_name | customer_email |
|------|------------------|--------|--------------|------------|---------|---------------|----------------|
| 2026-06-01 | Dawit Abebe | South | Laptop | 1 | 1200 | Alice Johnson | alice@mail.com |
| 2026-06-02 | Chaltu Kebede | Oromia | Phone | 2 | 1600 | Bob Smith | bob@mail.com |

**Part A: Identify problems with the current approach**

List at least 5 problems:

1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
4. _______________________________________________
5. _______________________________________________

**Part B: Design a relational database**

Create tables for:
* Salesperson
* Region
* Product
* Customer
* Sale
* SaleItem (if needed)

For each table:
* Primary Key
* Attributes
* Foreign Keys
* Relationships to other tables

**Part C: Map the old data to the new structure**

Show how one row from the spreadsheet would be distributed across your new tables.

**Original row:**

| date | salesperson_name | region | product_name | units_sold | revenue | customer_name | customer_email |
|------|------------------|--------|--------------|------------|---------|---------------|----------------|
| 2024-01-15 | Alice Johnson | West | Laptop Pro | 2 | 2400 | TechCorp | contact@techcorp.com |

**New tables:**
### 📊 Sales Database Schema (Empty Tables)

#### **1. TABLE: salespersons**

| salesperson_id (PK) | salesperson_name |
| :--- | :--- |
| | |

#### **2. TABLE: regions**

| region_id (PK) | region_name | salesperson_id (FK) |
| :--- | :--- | :--- |
| | | |

#### **3. TABLE: products**

| product_id (PK) | product_name | unit_price |
| :--- | :--- | :--- |
| | | |

#### **4. TABLE: customers**

| customer_id (PK) | customer_name | customer_email |
| :--- | :--- | :--- |
| | | |

#### **5. TABLE: sales**

| sale_id (PK) | date | customer_id (FK) | total_revenue |
| :--- | :--- | :--- | :--- |
| | | | |

#### **6. TABLE: sale_items**

| item_id (PK) | sale_id (FK) | product_id (FK) | units_sold | subtotal |
| :--- | :--- | :--- | :--- | :--- |
| | | | | |


**Part D: Benefits of the new design**

List 3 specific benefits the company will gain:

1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
