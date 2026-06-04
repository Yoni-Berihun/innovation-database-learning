# 🧪 Exercises — ER Modeling

Welcome to your ER modeling exercises! These are designed to help you practice the most important skill in database design: **translating real-world situations into visual blueprints**.

You don't need any special software — pen and paper work perfectly. If you want to try digital tools, check out dbdiagram.io or DrawSQL from the Learning Resources.

Take your time. Sketch freely. The goal is to build your design intuition.

---

## 🟢 Easy Exercises

These exercises help you identify entities and attributes — the building blocks of any ER model.

---

### Exercise 3.1: Spot the Entities

**Task:** Read each scenario below and list at least **4 entities** you would include in an ER model.

**Scenario A: A Pet Store**

1. ___________________
2. ___________________
3. ___________________
4. ___________________

**Scenario B: A Ride-Sharing App (like Uber)**

1. ___________________
2. ___________________
3. ___________________
4. ___________________

**Scenario C: A Streaming Service (like Netflix)**

1. ___________________
2. ___________________
3. ___________________
4. ___________________

**Scenario D: A Food Delivery App (like DoorDash)**

1. ___________________
2. ___________________
3. ___________________
4. ___________________

**Scenario E: Your University (the one you attend!)**

1. ___________________
2. ___________________
3. ___________________
4. ___________________

&gt; 💡 **Hint:** Entities are usually nouns — people, places, things, or events. If you can say "the system tracks many ______," it's probably an entity.

---

### Exercise 3.2: Attribute Hunter

**Task:** For each entity below, list at least **5 attributes** you would store.

**Entity: Student**

| # | Attribute Name | Example Value |
|---|---------------|---------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

**Entity: Course**

| # | Attribute Name | Example Value |
|---|---------------|---------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

**Entity: Instructor**

| # | Attribute Name | Example Value |
|---|---------------|---------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

**Entity: Department**

| # | Attribute Name | Example Value |
|---|---------------|---------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

---

### Exercise 3.3: Primary Key Practice

**Task:** For each entity below, suggest a good Primary Key. Explain why your choice is good.

| Entity | Suggested Primary Key | Why Is This a Good Choice? |
|--------|----------------------|---------------------------|
| Student | | |
| Course | | |
| Instructor | | |
| Department | | |
| Enrollment | | |
| Grade | | |

&gt; 💡 **Remember:** A good Primary Key is unique, never changes, and doesn't carry meaningful real-world data.

---

### Exercise 3.4: Entity or Attribute?

**Task:** For each item below, decide whether it should be an **entity** or an **attribute**. Explain your reasoning.

| Item | Entity or Attribute? | Why? |
|------|---------------------|------|
| A student's email address | | |
| A university department | | |
| A course's credit hours | | |
| A professor's office location | | |
| A grade received on an exam | | |
| A student's enrollment date | | |
| A library book's ISBN number | | |
| A customer's order history | | |

---

### Exercise 3.5: Complete the University Entity List

**Task:** Our University Management System has these entities: Department, Instructor, Student, Course, Enrollment, Grade.

For each entity, complete the following:

**a) What one instance of this entity represents** (e.g., "one student")

**b) Its Primary Key**

**c) Three attributes (besides the Primary Key)**

**d) Whether it has any Foreign Keys (and if so, to which entity)**

| Entity | One Instance Represents | Primary Key | Three Attributes | Has Foreign Keys? |
|--------|----------------------|-------------|------------------|-------------------|
| Department | | | | |
| Instructor | | | | |
| Student | | | | |
| Course | | | | |
| Enrollment | | | | |
| Grade | | | | |

---

## 🟡 Medium Exercises

These exercises focus on relationships — the connections that make a database truly powerful.

---

### Exercise 3.6: Relationship Detective

**Task:** For each pair of entities, identify the relationship type (One-to-One, One-to-Many, or Many-to-Many). Draw a simple arrow diagram.

**a) University System: Department and Instructor**

* Can one department have many instructors? _______
* Can one instructor work in many departments? _______
* Relationship type: _______

**Your diagram:**


---

**b) University System: Student and Course**

* Can one student take many courses? _______
* Can one course have many students? _______
* Relationship type: _______

**Your diagram:**


---

**c) University System: Instructor and Course**

* Can one instructor teach many courses? _______
* Can one course be taught by many instructors (in our simple model)? _______
* Relationship type: _______

**Your diagram:**


---

**d) Library System: Member and Book**

* Can one member borrow many books? _______
* Can one book be borrowed by many members (over time)? _______
* Relationship type: _______

**Your diagram:**


---

**e) Hospital System: Doctor and Patient**

* Can one doctor treat many patients? _______
* Can one patient see many doctors? _______
* Relationship type: _______

**Your diagram:**


---

### Exercise 3.7: Design the Relationships

**Task:** Design the complete relationship map for our University Management System.

**Step 1:** List all entities

1. ___________________
2. ___________________
3. ___________________
4. ___________________
5. ___________________
6. ___________________

**Step 2:** For each pair of entities, determine if they have a relationship

| Entity A | Entity B | Related? (Yes/No) | If Yes, What Type? |
|----------|----------|-------------------|-------------------|
| Department | Instructor | | |
| Department | Student | | |
| Department | Course | | |
| Instructor | Course | | |
| Instructor | Student | | |
| Student | Course | | |
| Student | Enrollment | | |
| Course | Enrollment | | |
| Enrollment | Grade | | |

**Step 3:** Draw your complete ER diagram

Use simple boxes and lines. Label each relationship with its type (1:1, 1:N, M:N).


---

### Exercise 3.8: Junction Entity Design

**Task:** In our university system, Students and Courses have a Many-to-Many relationship. We solve this with an Enrollment junction entity.

**Part A:** Design the Enrollment entity.

| Attribute | Type (PK/FK/Regular) | References Which Entity? |
|-----------|---------------------|-------------------------|
| | | |
| | | |
| | | |
| | | |

**Part B:** Answer these questions:

1. Why can't we just put `course_id` in the Student entity?
   _______________________________________________

2. Why can't we just put `student_id` in the Course entity?
   _______________________________________________

3. What additional information can we store in the Enrollment entity that doesn't belong in Student or Course?
   _______________________________________________

---

### Exercise 3.9: Foreign Key Placement

**Task:** In a One-to-Many relationship, the Foreign Key always goes on the "Many" side. Explain why this is, using the Department-Instructor relationship as an example.

**Your explanation:**

_______________________________________________
_______________________________________________
_______________________________________________

**Now apply this rule:** For each relationship below, state which entity should contain the Foreign Key.

| Relationship | Foreign Key Goes In | Column Name |
|--------------|---------------------|-------------|
| Department (1) → Instructors (N) | | |
| Instructor (1) → Courses (N) | | |
| Student (1) → Enrollments (N) | | |
| Course (1) → Enrollments (N) | | |
| Enrollment (1) → Grades (N) | | |

---

### Exercise 3.10: Draw a Complete ER Diagram

**Task:** Draw a complete ER diagram for a **simple online store** that sells books.

**Entities to include:**
* Customer
* Book
* Order
* OrderItem (junction between Order and Book)
* Author

**Requirements:**
1. Each entity must have a Primary Key and at least 3 attributes
2. Show all relationships with correct types
3. Show all Foreign Keys
4. Include a junction entity for any Many-to-Many relationships

**Your diagram:**

---

## 🔴 Challenging Exercises

These exercises present realistic scenarios that require full ER model design. They test your ability to identify entities, attributes, keys, and relationships in complex systems.

---

### Exercise 3.11: Hospital Management System

**Task:** Design a complete ER model for a medium-sized hospital.

**System requirements:**
* The hospital has multiple departments (Emergency, Cardiology, Pediatrics, etc.)
* Each department has many doctors
* Each doctor belongs to one department
* Patients register at the hospital and have medical records
* Patients make appointments with doctors
* Each appointment results in a medical record entry
* Medical records include diagnoses, prescriptions, and notes
* The hospital has rooms that patients can be assigned to
* Nurses are assigned to departments and assist doctors
* Medications are prescribed during appointments

**Part A: Identify all entities** (at least 8)

1. ___________________
2. ___________________
3. ___________________
4. ___________________
5. ___________________
6. ___________________
7. ___________________
8. ___________________

**Part B: Define attributes for each entity** (at least 4 per entity, including Primary Key)

**Part C: Identify all relationships** (use a table)

| Entity A | Relationship | Entity B | Type |
|----------|-------------|----------|------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Part D: Draw the complete ER diagram**


**Part E: Design challenges**

1. A user can follow many users, and many users can follow one user. How do you model this without creating duplicate data?
   _______________________________________________

2. A post can have many hashtags, and a hashtag can appear on many posts. What type of relationship is this, and how do you implement it?
   _______________________________________________

3. How would you prevent a user from liking the same post twice?
   _______________________________________________

---

### Exercise 3.13: Complete University Management System ER Model

**Task:** This is the capstone exercise for this module. Design the complete ER model for our University Management System — the same one we'll build in PostgreSQL in upcoming modules.

**Required entities:**
* Department
* Instructor
* Student
* Course
* Enrollment
* Grade

**Requirements:**

**Part A: For each entity, define:**
* Purpose (one sentence)
* Primary Key
* At least 6 attributes
* Data type for each attribute (your best guess)
* Which attributes are Foreign Keys (if any)

**Department**

| Attribute | Data Type | PK/FK/Regular | Notes |
|-----------|-----------|---------------|-------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Instructor**

| Attribute | Data Type | PK/FK/Regular | Notes |
|-----------|-----------|---------------|-------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Student**

| Attribute | Data Type | PK/FK/Regular | Notes |
|-----------|-----------|---------------|-------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Course**

| Attribute | Data Type | PK/FK/Regular | Notes |
|-----------|-----------|---------------|-------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Enrollment**

| Attribute | Data Type | PK/FK/Regular | Notes |
|-----------|-----------|---------------|-------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Grade**

| Attribute | Data Type | PK/FK/Regular | Notes |
|-----------|-----------|---------------|-------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

**Part B: Relationship map**

Complete this table:

| Entity A | Relationship Verb | Entity B | Cardinality | Foreign Key Location |
|----------|-------------------|----------|-------------|---------------------|
| Department | has | Instructors | 1:N | instructors.department_id |
| Department | offers | Courses | | |
| Instructor | teaches | Courses | | |
| Student | makes | Enrollments | | |
| Course | receives | Enrollments | | |
| Enrollment | has | Grades | | |

**Part C: Complete ER diagram**

Draw the full diagram with all entities, attributes, keys, and relationships.


**Part D: Validation questions**

1. Can a course exist without an instructor assigned? Should it be allowed?
   _______________________________________________

2. Can a student exist without any enrollments? Is this realistic?
   _______________________________________________

3. If we delete a student, what should happen to their enrollments and grades?
   _______________________________________________

4. What if two students have the same name? How does your design prevent confusion?
   _______________________________________________

5. Draw the path to answer this question: "What is the name of the department that offers the course 'Introduction to Programming'?"
   _______________________________________________

   
