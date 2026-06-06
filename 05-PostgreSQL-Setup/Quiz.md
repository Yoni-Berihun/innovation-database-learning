
# ✅ Quiz — PostgreSQL Setup & First Steps

Test your understanding before moving to the next module. There are 10 questions. Take your time.

---

## Question 1 (Multiple Choice)

What is PostgreSQL?

- [ ] A programming language for building websites
- [ ] A free, open-source relational database management system
- [ ] A spreadsheet application like Excel
- [ ] A text editor for writing code

---

## Question 2 (Multiple Choice)

Which of these companies uses PostgreSQL?

- [ ] Instagram
- [ ] Spotify
- [ ] Reddit
- [ ] All of the above

---

## Question 3 (Multiple Choice)

What is pgAdmin?

- [ ] A programming language
- [ ] A graphical tool for managing PostgreSQL databases
- [ ] A type of database server
- [ ] A web browser

---

## Question 4 (Multiple Choice)

What is the default port number for PostgreSQL?

- [ ] 3306
- [ ] 5432
- [ ] 8080
- [ ] 3000

---

## Question 5 (Multiple Choice)

During installation, you set a password for which user?

- [ ] admin
- [ ] root
- [ ] postgres
- [ ] user

---

## Question 6 (Multiple Choice)

What does this query do? `SELECT 10 + 5;`

- [ ] Creates a table with 15 rows
- [ ] Adds 10 and 5 and returns the result
- [ ] Deletes 15 records
- [ ] Shows 10 tables and 5 columns

---

## Question 7 (Short Answer)

Why is PostgreSQL a good choice for students who want to learn databases and eventually work professionally?

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 8 (Short Answer)

You open pgAdmin and see a red dot next to your server. What does this mean, and what is the first thing you should check?

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 9 (Short Answer)

Explain the difference between PostgreSQL and pgAdmin. What does each one do?

_______________________________________________
_______________________________________________
_______________________________________________

---

## Question 10 (Short Answer)

You ran this query: `SELECT current_date;`

What result would you expect to see, and what does this query demonstrate about PostgreSQL?

_______________________________________________
_______________________________________________
_______________________________________________

---

## ✅ Answer Key

<details>
<summary>Click to reveal answers and explanations</summary>

### Question 1
**Correct answer:** A free, open-source relational database management system

**Why:** PostgreSQL is a database management system (DBMS), not a programming language, spreadsheet, or text editor. It stores and manages data using the relational model.

---

### Question 2
**Correct answer:** All of the above

**Why:** Instagram, Spotify, and Reddit all use PostgreSQL. Many other major companies including Netflix, Uber, and Airbnb also rely on PostgreSQL.

---

### Question 3
**Correct answer:** A graphical tool for managing PostgreSQL databases

**Why:** pgAdmin is the graphical user interface (GUI) that comes with PostgreSQL. It lets you create databases, run queries, and manage your database visually without memorizing commands.

---

### Question 4
**Correct answer:** 5432

**Why:** 5432 is the default port for PostgreSQL. MySQL uses 3306, 8080 is common for web servers, and 3000 is often used for development applications.

---

### Question 5
**Correct answer:** postgres

**Why:** The default superuser account in PostgreSQL is named "postgres". You set a password for this account during installation. This is the most important password to remember.

---

### Question 6
**Correct answer:** Adds 10 and 5 and returns the result

**Why:** `SELECT` retrieves data. In this case, it retrieves the result of the mathematical expression 10 + 5, which is 15. PostgreSQL can perform calculations directly in queries.

---

### Question 7
**Sample good answer:**

PostgreSQL is a good choice because it is free and open-source, so students can learn without cost. It follows SQL standards closely, so skills transfer to other databases. It is used by major companies, so learning it provides job-ready skills. It is reliable, well-documented, and has a large community for support.

*(Any answer mentioning free/open-source, SQL standards, real-world usage, or job relevance receives full credit.)*

---

### Question 8
**Sample good answer:**

A red dot means pgAdmin cannot connect to the PostgreSQL server. The first thing to check is whether the PostgreSQL service is running. On Windows, check Services; on Mac, check if the service started; on Linux, use `sudo systemctl status postgresql`. If the service is stopped, start it.

*(Any answer mentioning the service status and how to check it receives full credit.)*

---

### Question 9
**Sample good answer:**

PostgreSQL is the actual database server — the software that stores data, processes queries, and manages everything. pgAdmin is a graphical tool that connects to PostgreSQL and lets you interact with it visually. You can think of PostgreSQL as the engine and pgAdmin as the dashboard.

*(Any clear distinction between the database server and the management tool receives full credit.)*

---

### Question 10
**Sample good answer:**

You would expect to see today's date (for example, 2024-06-05). This query demonstrates that PostgreSQL has built-in functions that know current information like the date and time. It shows that a database can do more than just store data — it can also process and return dynamic information.

*(Any answer mentioning today's date and the concept of built-in functions receives full credit.)*

</details>



> 💬 **Remember:** Having PostgreSQL installed and running on your computer is a real achievement. You now have the same professional database tool used by engineers at the world's biggest tech companies. The next modules will teach you to build tables, insert data, and write powerful queries. Your database journey is just beginning! 🚀
