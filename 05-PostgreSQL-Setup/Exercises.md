
# 🧪 Exercises — PostgreSQL Setup & First Steps

These exercises help you verify your installation, get comfortable with pgAdmin, and build confidence running your first SQL commands.

---

## 🟢 Easy Exercises

### Exercise 5.1: Verify Your Installation

**Task:** Confirm PostgreSQL is properly installed.

1. Open pgAdmin
2. Connect to your PostgreSQL server
3. Check that the server shows a green dot

**Questions:**

a) What version of PostgreSQL did you install? ___________________

b) What is the name of your server in pgAdmin? ___________________

c) Does the server show a green dot (connected) or red dot (disconnected)? ___________________

---

### Exercise 5.2: Explore the Interface

**Task:** Click around pgAdmin and identify these items.

a) Where do you find the list of databases? ___________________

b) How do you open the Query Tool? ___________________

c) What button runs your SQL query? (Hint: It looks like a lightning bolt.) ___________________

d) What keyboard shortcut runs a query? ___________________

---

### Exercise 5.3: Run Your First Queries

**Task:** Open the Query Tool for `university_db` and run each query below. Write down what you see.

a) `SELECT 'PostgreSQL is working!';`

Result: ___________________

b) `SELECT 25 * 4;`

Result: ___________________

c) `SELECT 100 / 4;`

Result: ___________________

d) `SELECT current_date;`

Result: ___________________

---

### Exercise 5.4: Create a Practice Database

**Task:** Create a new database called `my_first_db`.

1. Right-click on Databases
2. Select Create → Database
3. Name it `my_first_db`
4. Click Save

**Questions:**

a) Did you see `my_first_db` appear in the database list? ___________________

b) Open the Query Tool for `my_first_db` and run: `SELECT 'This is my first database!';`

Result: ___________________

---

### Exercise 5.5: Check Your Tools

**Task:** Make sure you can find all these tools.

| Tool | Where to Find It | Did You Find It? (Yes/No) |
|------|-----------------|--------------------------|
| pgAdmin | | |
| Query Tool | | |
| Server status (green/red dot) | | |
| Database creation menu | | |
| Execute button (lightning bolt) | | |

---

## 🟡 Medium Exercises

### Exercise 5.6: Create Multiple Databases

**Task:** Create three databases for different purposes.

1. `university_db` — for our course (if not already created)
2. `library_db` — for a library system
3. `shop_db` — for a small online store

**For each database:**

a) Create it in pgAdmin
b) Open the Query Tool
c) Run: `SELECT current_database();`

This command shows which database you're currently connected to.

**Questions:**

| Database | Result of `SELECT current_database();` |
|----------|--------------------------------------|
| university_db | |
| library_db | |
| shop_db | |

---

### Exercise 5.7: Experiment with SQL

**Task:** Run these queries in `university_db` and observe the results.

a) `SELECT 7 + 8 * 2;`

Result: ___________________

Does SQL follow order of operations (multiplication before addition)? ___________________

b) `SELECT 'Hello' || ' ' || 'World';`

Result: ___________________

What does `||` do? ___________________

c) `SELECT LENGTH('PostgreSQL');`

Result: ___________________

What does `LENGTH()` do? ___________________

d) `SELECT UPPER('i am shouting');`

Result: ___________________

What does `UPPER()` do? ___________________

---

### Exercise 5.8: Find Your PostgreSQL Version

**Task:** Run this query in any database:

```sql
SELECT version();
