# 📚 Complete Learning Resources for PostgreSQL Mentorship Course

# Modules 1–18 of the PostgreSQL Mentorship Course  
&gt; **Last Updated:** June 2026

---

## Table of Contents
1. [Video Learning](#-video-learning)
2. [Official Documentation](#-official-documentation)
3. [Interactive Practice Platforms](#-interactive-practice-platforms)
4. [Cheat Sheets & Quick References](#-cheat-sheets--quick-references)
5. [Books & Long-Form Guides](#-books--long-form-guides)
6. [Community & Help](#-community--help)
7. [Tools & Software](#-tools--software)
8. [Sample Datasets & Projects](#-sample-datasets--projects)
9. [Interview Preparation](#-interview-preparation)
10. [PostgreSQL-Specific Resources](#-postgresql-specific-resources)

---

## 🎥 Video Learning

### Beginner-Friendly Playlists

| Title | Channel | Why Useful | Link |
|-------|---------|------------|------|
| **PostgreSQL Full Course** | freeCodeCamp | Comprehensive 8-hour course covering installation to advanced features; perfect for visual learners who want one complete resource | [YouTube](https://www.youtube.com/freeCodeCamp) |
| **SQL Tutorial for Beginners** | Programming with Mosh | Clear, slow-paced explanations of every core concept; ideal for students who need to build confidence before moving to advanced topics | [YouTube](https://www.youtube.com/programmingwithmosh) |
| **PostgreSQL Advanced Features** | Amigoscode | Project-based walkthroughs of CTEs, window functions, and triggers; shows how features work together in real applications | [YouTube](https://www.youtube.com/amigoscode) |
| **SQL Full Course** | Bro Code | Fun, engaging style that keeps beginners motivated; covers all fundamentals with plenty of examples | [YouTube](https://www.youtube.com/brocodez) |
| **Database Design & Normalization** | Caleb Curry | Deep dive into ER modeling, normalization forms, and schema design decisions you will face in university projects | [YouTube](https://www.youtube.com) |
| **SQL Window Functions Explained** | Fireship | 10-minute visual explanation of ranking and analytical functions; great for quick review before exams or interviews | [YouTube](https://www.youtube.com) |
| **How Do SQL Indexes Work?** | Hussein Nasser | Visual explanation of B-trees and index internals; helps you understand *why* indexes speed up queries, not just *how* | [YouTube](https://www.youtube.com) |
| **SQL Transactions & ACID** | freeCodeCamp | Detailed explanation of transaction isolation, deadlocks, and concurrency; essential for understanding multi-user systems | [YouTube](https://www.youtube.com/freeCodeCamp) |
| **Database Optimization Explained** | Amigoscode | Real-world performance tuning scenarios; shows how slow queries become fast with proper indexing and query rewriting | [YouTube](https://www.youtube.com/amigoscode) |
| **PL/pgSQL Tutorial** | Programming with Mosh | Introduction to stored procedures, functions, and triggers in PostgreSQL's procedural language | [YouTube](https://www.youtube.com/programmingwithmosh) |

---

## 📄 Official Documentation

### Core PostgreSQL Documentation

| Topic | Link | Why Useful |
|-------|------|------------|
| **PostgreSQL 16 Release Notes** | [postgresql.org/docs/16/release-16.html](https://www.postgresql.org/docs/16/release-16.html) | See what's new in the latest version; helpful for understanding modern features |
| **SQL Commands** | [postgresql.org/docs/16/sql-commands.html](https://www.postgresql.org/docs/16/sql-commands.html) | Complete reference for every SQL command in PostgreSQL |
| **SELECT** | [postgresql.org/docs/16/sql-select.html](https://www.postgresql.org/docs/16/sql-select.html) | The most-used command; reference for all clauses, joins, and options |
| **INSERT** | [postgresql.org/docs/16/sql-insert.html](https://www.postgresql.org/docs/16/sql-insert.html) | Syntax for adding data, including RETURNING clause and ON CONFLICT |
| **UPDATE** | [postgresql.org/docs/16/sql-update.html](https://www.postgresql.org/docs/16/sql-update.html) | Modifying existing data safely |
| **DELETE** | [postgresql.org/docs/16/sql-delete.html](https://www.postgresql.org/docs/16/sql-delete.html) | Removing data, including USING clause for joins |
| **CREATE TABLE** | [postgresql.org/docs/16/sql-createtable.html](https://www.postgresql.org/docs/16/sql-createtable.html) | Defining tables with all constraint types |
| **ALTER TABLE** | [postgresql.org/docs/16/sql-altertable.html](https://www.postgresql.org/docs/16/sql-altertable.html) | Modifying existing table structure |
| **DROP TABLE** | [postgresql.org/docs/16/sql-droptable.html](https://www.postgresql.org/docs/16/sql-droptable.html) | Removing tables safely |
| **Data Types** | [postgresql.org/docs/16/datatype.html](https://www.postgresql.org/docs/16/datatype.html) | Complete guide to all built-in data types |
| **Constraints** | [postgresql.org/docs/16/ddl-constraints.html](https://www.postgresql.org/docs/16/ddl-constraints.html) | NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK, EXCLUDE |
| **Indexes** | [postgresql.org/docs/16/indexes-types.html](https://www.postgresql.org/docs/16/indexes-types.html) | B-tree, Hash, GIN, GiST, BRIN, and when to use each |
| **CREATE INDEX** | [postgresql.org/docs/16/sql-createindex.html](https://www.postgresql.org/docs/16/sql-createindex.html) | Syntax and options for creating indexes |
| **EXPLAIN** | [postgresql.org/docs/16/sql-explain.html](https://www.postgresql.org/docs/16/sql-explain.html) | Understanding query execution plans |
| **VACUUM** | [postgresql.org/docs/16/sql-vacuum.html](https://www.postgresql.org/docs/16/sql-vacuum.html) | Cleaning up dead rows and reclaiming space |
| **ANALYZE** | [postgresql.org/docs/16/sql-analyze.html](https://www.postgresql.org/docs/16/sql-analyze.html) | Updating table statistics for query planning |
| **Transactions** | [postgresql.org/docs/16/tutorial-transactions.html](https://www.postgresql.org/docs/16/tutorial-transactions.html) | BEGIN, COMMIT, ROLLBACK, SAVEPOINT |
| **Transaction Isolation** | [postgresql.org/docs/16/transaction-iso.html](https://www.postgresql.org/docs/16/transaction-iso.html) | Read Committed, Repeatable Read, Serializable |
| **WITH Queries (CTEs)** | [postgresql.org/docs/16/queries-with.html](https://www.postgresql.org/docs/16/queries-with.html) | Common Table Expressions syntax and examples |
| **Window Functions** | [postgresql.org/docs/16/tutorial-window.html](https://www.postgresql.org/docs/16/tutorial-window.html) | Complete window function tutorial |
| **CREATE FUNCTION** | [postgresql.org/docs/16/sql-createfunction.html](https://www.postgresql.org/docs/16/sql-createfunction.html) | Writing stored functions in SQL and PL/pgSQL |
| **CREATE PROCEDURE** | [postgresql.org/docs/16/sql-createprocedure.html](https://www.postgresql.org/docs/16/sql-createprocedure.html) | Creating stored procedures |
| **CREATE TRIGGER** | [postgresql.org/docs/16/sql-createtrigger.html](https://www.postgresql.org/docs/16/sql-createtrigger.html) | Automating actions on table events |
| **CREATE VIEW** | [postgresql.org/docs/16/sql-createview.html](https://www.postgresql.org/docs/16/sql-createview.html) | Creating virtual tables |
| **CREATE MATERIALIZED VIEW** | [postgresql.org/docs/16/sql-creatematerializedview.html](https://www.postgresql.org/docs/16/sql-creatematerializedview.html) | Caching query results for performance |
| **PL/pgSQL Language** | [postgresql.org/docs/16/plpgsql.html](https://www.postgresql.org/docs/16/plpgsql.html) | Complete guide to PostgreSQL's procedural language |
| **Performance Tips** | [postgresql.org/docs/16/performance-tips.html](https://www.postgresql.org/docs/16/performance-tips.html) | Official optimization recommendations |
| **Server Configuration** | [postgresql.org/docs/16/runtime-config.html](https://www.postgresql.org/docs/16/runtime-config.html) | Tuning PostgreSQL for your hardware (advanced) |
| **Backup and Restore** | [postgresql.org/docs/16/backup.html](https://www.postgresql.org/docs/16/backup.html) | pg_dump, pg_restore, and continuous archiving |
| **Security** | [postgresql.org/docs/16/security.html](https://www.postgresql.org/docs/16/security.html) | Roles, privileges, and row-level security |

---

## 🧪 Interactive Practice Platforms

### Free Platforms

| Platform | Best For | Why Useful | Link |
|----------|----------|------------|------|
| **pgExercises** | PostgreSQL-specific practice | Exercises match your course topics exactly; includes solutions and explanations for every problem | [pgexercises.com](https://pgexercises.com) |
| **SQLZoo** | Beginner fundamentals | University-themed problems with immediate feedback; great for building muscle memory on basic syntax | [sqlzoo.net](https://sqlzoo.net) |
| **SQLBolt** | Interactive lessons | No setup required; learn in your browser with guided lessons from SELECT to JOINs | [sqlbolt.com](https://sqlbolt.com) |
| **W3Schools SQL** | Quick reference + practice | Try-it editor lets you test commands instantly; excellent for checking syntax | [w3schools.com/sql](https://www.w3schools.com/sql) |
| **Mode Analytics SQL Tutorial** | Real datasets | Learn SQL using real-world data in a professional analytics environment | [mode.com/sql-tutorial](https://mode.com/sql-tutorial) |
| **SQLite Online** | Quick testing | Test SQL snippets instantly without installing anything; supports multiple database flavors | [sqliteonline.com](https://sqliteonline.com) |
| **DB Fiddle** | Schema testing | Shareable database schemas and queries; great for asking questions on Stack Overflow | [dbfiddle.uk](https://dbfiddle.uk) |

### Gamified / Challenge Platforms

| Platform | Best For | Why Useful | Link |
|----------|----------|------------|------|
| **HackerRank SQL** | Gamified challenges | Earn points and badges while solving problems from easy to hard; good for motivation | [hackerrank.com/domains/sql](https://www.hackerrank.com/domains/sql) |
| **LeetCode Database** | Interview preparation | Real interview questions from tech companies; excellent for internship and job prep | [leetcode.com/problemset/database](https://leetcode.com/problemset/database) |
| **DataLemur** | Company-specific questions | SQL interview questions from companies like Meta, Amazon, and Google | [datalemur.com](https://datalemur.com) |
| **StrataScratch** | Data science SQL | Business-case SQL problems; great for analytics and data engineering roles | [stratascratch.com](https://www.stratascratch.com) |
| **Codewars SQL** | Community challenges | Kata-style coding challenges created by the community; fun and competitive | [codewars.com](https://www.codewars.com) |

### Paid Platforms (Free Trials Available)

| Platform | Best For | Why Useful | Link |
|----------|----------|------------|------|
| **DataCamp** | Structured courses | Guided learning paths with video + interactive exercises; excellent for beginners who want structure | [datacamp.com](https://www.datacamp.com) |
| **Udemy** | Affordable courses | Frequent sales ($10-15 courses); many PostgreSQL-specific options with lifetime access | [udemy.com](https://www.udemy.com) |
| **Coursera** | University courses | SQL courses from universities like Duke and UC Davis; certificates available | [coursera.org](https://www.coursera.org) |
| **Pluralsight** | Professional skills | Deep dives into PostgreSQL administration and performance tuning | [pluralsight.com](https://www.pluralsight.com) |
| **LinkedIn Learning** | Career-focused | Short courses that fit into a lunch break; integrates with your LinkedIn profile | [linkedin.com/learning](https://www.linkedin.com/learning) |

---

## 📋 Cheat Sheets & Quick References

### PostgreSQL Cheat Sheets

| Resource | Why Useful | Link |
|----------|------------|------|
| **PostgreSQL Cheat Sheet** by PostgreSQL Tutorial | One-page reference for common commands, data types, and functions | [postgresqltutorial.com/postgresql-cheat-sheet](https://www.postgresqltutorial.com/postgresql-cheat-sheet/) |
| **PostgreSQL psql Commands** | Essential psql meta-commands for command-line work | [postgresqltutorial.com/postgresql-cheat-sheet](https://www.postgresqltutorial.com/postgresql-cheat-sheet/) |
| **SQL Cheat Sheet** by W3Schools | Quick lookup for basic SQL commands across all databases | [w3schools.com/sql/sql_quickref.asp](https://www.w3schools.com/sql/sql_quickref.asp) |
| **SQL Joins Visual Cheat Sheet** | Venn diagram reference for INNER, LEFT, RIGHT, FULL joins | Search "SQL joins cheat sheet" |
| **Window Functions Cheat Sheet** by pgMustard | Visual guide to window function syntax and use cases | [pgmustard.com](https://pgmustard.com/) |
| **PostgreSQL Data Types Reference** | Quick comparison of all PostgreSQL data types | [postgresql.org/docs/16/datatype.html](https://www.postgresql.org/docs/16/datatype.html) |
| **Regular Expressions in PostgreSQL** | Pattern matching cheat sheet for text searches | [postgresql.org/docs/16/functions-matching.html](https://www.postgresql.org/docs/16/functions-matching.html) |
| **Date/Time Functions Cheat Sheet** | Quick reference for PostgreSQL's powerful date/time functions | [postgresql.org/docs/16/functions-datetime.html](https://www.postgresql.org/docs/16/functions-datetime.html) |

### Printable PDFs

| Resource | Why Useful | Link |
|----------|------------|------|
| **PostgreSQL 16 Cheat Sheet PDF** | Downloadable, printable reference for offline use | Search "PostgreSQL 16 cheat sheet PDF" |
| **SQL Basics PDF** by SQLBolt | Printable version of all SQLBolt lessons | [sqlbolt.com](https://sqlbolt.com) |

---

## 📚 Books & Long-Form Guides

### Free Books

| Title | Author | Why Useful | Link |
|-------|--------|------------|------|
| **The Internals of PostgreSQL** | Hironobu Suzuki | Deep dive into how PostgreSQL works internally; excellent for curious students who want to understand the engine | [interdb.jp/pg](https://www.interdb.jp/pg/) |
| **PostgreSQL: Up and Running** | Regina Obe & Leo Hsu | Practical guide for getting PostgreSQL installed and configured correctly | [amazon.com](https://www.amazon.com) |
| **Mastering PostgreSQL** | Dimitri Fontaine | Comprehensive guide from basics to advanced administration | [masteringpostgresql.com](https://masteringpostgresql.com) |

### Free Online Guides

| Title | Why Useful | Link |
|-------|------------|------|
| **Use The Index, Luke!** by Markus Winand | The best free resource for understanding SQL indexing and query optimization; interactive examples | [use-the-index-luke.com](https://use-the-index-luke.com) |
| **PostgreSQL Tutorial** | Step-by-step tutorials covering every topic in this course with working examples | [postgresqltutorial.com](https://www.postgresqltutorial.com) |
| **TutorialsPoint PostgreSQL** | Alternative explanations and examples; good for comparing explanations when stuck | [tutorialspoint.com/postgresql](https://www.tutorialspoint.com/postgresql) |
| **GeeksforGeeks PostgreSQL** | Quick explanations with code snippets; great for last-minute exam review | [geeksforgeeks.org/postgresql](https://www.geeksforgeeks.org/postgresql) |

### Paid Books (Worth the Investment)

| Title | Why Useful | Link |
|-------|------------|------|
| **PostgreSQL 16 Administration Cookbook** | Solutions to real-world administration problems | [packtpub.com](https://www.packtpub.com) |
| **High Performance PostgreSQL for Rails** | Database optimization specifically for web developers | [pragprog.com](https://pragprog.com) |
| **SQL Antipatterns** by Bill Karwin | Learn what NOT to do; essential for avoiding common mistakes | [pragprog.com](https://pragprog.com) |

---

## 💬 Community & Help

### Forums & Q&A

| Community | Why Useful | Link |
|-----------|------------|------|
| **r/PostgreSQL** | Active Reddit community for PostgreSQL questions, news, and discussions | [reddit.com/r/PostgreSQL](https://reddit.com/r/PostgreSQL) |
| **r/SQL** | General SQL discussions; good for conceptual questions and career advice | [reddit.com/r/SQL](https://reddit.com/r/SQL) |
| **r/learnprogramming** | Beginner-friendly programming community; SQL questions welcome | [reddit.com/r/learnprogramming](https://reddit.com/r/learnprogramming) |
| **Stack Overflow [postgresql]** | The go-to place for specific technical questions; search before asking | [stackoverflow.com/questions/tagged/postgresql](https://stackoverflow.com/questions/tagged/postgresql) |
| **Stack Overflow [sql]** | Broader SQL questions applicable to multiple databases | [stackoverflow.com/questions/tagged/sql](https://stackoverflow.com/questions/tagged/sql) |
| **Database Administrators Stack Exchange** | Advanced questions about database design, administration, and performance | [dba.stackexchange.com](https://dba.stackexchange.com) |

### Real-Time Chat

| Community | Why Useful | Link |
|-----------|------------|------|
| **PostgreSQL Discord** | Real-time chat with PostgreSQL professionals and enthusiasts | Search "PostgreSQL Discord" |
| **The Coding Den Discord** | General programming community with SQL channels | Search "The Coding Den Discord" |
| **Devcord Discord** | Developer community with database discussion channels | Search "Devcord Discord" |

### Local & University Resources

| Resource | Why Useful |
|----------|------------|
| **University Computer Science Department** | Office hours, TA sessions, and study groups |
| **CS/IS Student Organizations** | Peer study groups and project collaboration |
| **Career Center** | Resume reviews and internship connections |

---

## 🛠️ Tools & Software

### Database Clients

| Tool | Platform | Why Useful | Link |
|------|----------|------------|------|
| **pgAdmin 4** | Windows, Mac, Linux | Official PostgreSQL GUI; comprehensive tool for beginners and professionals | [pgadmin.org](https://www.pgadmin.org) |
| **DBeaver** | Windows, Mac, Linux | Universal database tool supporting PostgreSQL and 80+ other databases; free community edition | [dbeaver.io](https://dbeaver.io) |
| **DataGrip** | Windows, Mac, Linux | JetBrains IDE for databases; intelligent code completion and refactoring | [jetbrains.com/datagrip](https://www.jetbrains.com/datagrip/) |
| **TablePlus** | Windows, Mac, Linux | Modern, native database GUI; clean interface perfect for beginners | [tableplus.com](https://tableplus.com) |
| **Postico** | Mac only | Beautiful, simple PostgreSQL client for macOS users | [eggerapps.at/postico](https://eggerapps.at/postico) |
| **psql** | Command line | PostgreSQL's built-in command-line tool; essential for server administration | Included with PostgreSQL |

### Schema Design Tools

| Tool | Why Useful | Link |
|------|------------|------|
| **dbdiagram.io** | Draw database diagrams and export SQL; free for basic use | [dbdiagram.io](https://dbdiagram.io) |
| **draw.io (diagrams.net)** | Free, open-source diagramming tool; great for ER diagrams | [draw.io](https://draw.io) |
| **Lucidchart** | Professional diagramming with collaboration features | [lucidchart.com](https://www.lucidchart.com) |
| **ERDPlus** | Free tool specifically for entity-relationship diagrams | [erdplus.com](https://erdplus.com) |

### Performance & Monitoring Tools

| Tool | Why Useful | Link |
|------|------------|------|
| **pgMustard** | Analyzes EXPLAIN ANALYZE output and suggests optimizations | [pgmustard.com](https://pgmustard.com) |
| **pgBadger** | Generates detailed performance reports from PostgreSQL logs | [pgbadger.darold.net](https://pgbadger.darold.net) |
| **PgHero** | Open-source dashboard for PostgreSQL performance metrics | [github.com/ankane/pghero](https://github.com/ankane/pghero) |
| **EXPLAIN Analyze Visualizer** | Visual representation of query plans | [explain.dalibo.com](https://explain.dalibo.com) |

---

## 🗃️ Sample Datasets & Projects

### University Management System Dataset

The complete sample dataset used throughout this course:

```sql
-- ============================================
-- UNIVERSITY MANAGEMENT SYSTEM SAMPLE DATA
-- ============================================

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
