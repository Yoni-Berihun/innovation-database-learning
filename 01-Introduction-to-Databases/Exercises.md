
---


# 🧪 Exercises — Introduction to Databases

Welcome to your first set of exercises! Don't worry — there's no coding yet. These exercises are designed to help you *think* like a database person. The best database professionals aren't just good at typing commands; they're good at understanding how information should be organized.

Take your time. There are no deadlines here. The goal is understanding, not speed.

---

## 🟢 Easy Exercises

These exercises reinforce the core concepts. If you can do these, you understand the basics.

---

### Exercise 1.1: Define It Yourself

**Task:** In your own words (no copying from the module!), explain what a database is. Imagine you're explaining it to a friend who has never heard the word before.

**Your answer:**

_______________________________________________
_______________________________________________
_______________________________________________

**Now explain it to a 10-year-old:**

_______________________________________________
_______________________________________________
_______________________________________________

> 💡 **Tip:** Using an analogy helps! Maybe compare it to a library, a filing cabinet, or something from your own life.

---

### Exercise 1.2: Data Detective

**Task:** Look at your phone's home screen. Pick **three apps** you use regularly. For each app, list at least **four pieces of data** you believe the app stores about you or its content.

**App 1: ___________________**

| # | Data Stored | Why the App Needs It |
|---|-------------|----------------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |

**App 2: ___________________**

| # | Data Stored | Why the App Needs It |
|---|-------------|----------------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |

**App 3: ___________________**

| # | Data Stored | Why the App Needs It |
|---|-------------|----------------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |

---

### Exercise 1.3: Database or Not a Database?

**Task:** For each scenario below, decide whether a database is the right tool. Explain your reasoning.

| Scenario | Database? (Yes/No) | Why? |
|----------|-------------------|------|
| A student tracking their personal homework in a notebook | | |
| Amazon storing 300 million customer accounts and order histories | | |
| A small bakery writing daily sales in a paper ledger | | |
| A hospital storing patient records for 50,000 people | | |
| You making a grocery list on your phone's notes app | | |
| Netflix recommending shows based on your watch history | | |
| A university managing 20,000 students, 1,000 courses, and 50,000 enrollments | | |

---

### Exercise 1.4: The University Data Hunt

**Task:** Think about your own university experience. List **10 pieces of data** your university definitely stores about you or the academic environment.

**Example:** My student ID number, my enrolled courses, my grades, my tuition payment status...

1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
4. _______________________________________________
5. _______________________________________________
6. _______________________________________________
7. _______________________________________________
8. _______________________________________________
9. _______________________________________________
10. _______________________________________________

---

## 🟡 Medium Exercises

These exercises ask you to apply what you've learned to new situations. They combine concepts and require a bit more thinking.

---

### Exercise 1.5: The Spreadsheet Disaster

**Task:** Read the scenario below and answer the questions.

**Scenario:** Professor Martinez teaches Introduction to Programming. She keeps all her student grades in a single Excel spreadsheet. The file is stored on her laptop. One day:

1. Her laptop crashes. The spreadsheet file is corrupted.
2. She had emailed a copy to the department admin last month, but it's missing recent grades.
3. Two students claim they submitted assignments that aren't in the spreadsheet.
4. The department chair needs a report of all students with a GPA below 2.0, but the spreadsheet has no GPA calculation.
5. Professor Martinez is on sick leave, and no one else can access her laptop.

**Questions:**

**a)** List **five problems** with using a spreadsheet for this situation.

1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
4. _______________________________________________
5. _______________________________________________

**b)** For each problem, explain how a database would solve it.

| Problem | How a Database Solves It |
|---------|------------------------|
| 1 | |
| 2 | |
| 3 | |
| 4 | |
| 5 | |

**c)** What specific database features would help here? (Hint: Think about backups, multiple users, validation, calculations, and access control.)

_______________________________________________
_______________________________________________
_______________________________________________

---

### Exercise 1.6: Design a Simple Library System

**Task:** Imagine you're designing a database for a **public library**. The library needs to track:

* Books
* Members (people who borrow books)
* Loans (who borrowed what, and when)
* Authors
* Categories (Fiction, Science, History, etc.)

**Part A:** List the **tables** you would need. For each table, list at least **4 columns** (pieces of information) you would store.

**Table 1: ___________________**

| Column Name | What It Stores | Example Value |
|-------------|---------------|---------------|
| | | |
| | | |
| | | |
| | | |

**Table 2: ___________________**

| Column Name | What It Stores | Example Value |
|-------------|---------------|---------------|
| | | |
| | | |
| | | |
| | | |

**Table 3: ___________________**

| Column Name | What It Stores | Example Value |
|-------------|---------------|---------------|
| | | |
| | | |
| | | |
| | | |

*(Continue for all tables you identified)*

**Part B:** Think about how these tables connect. Draw arrows or write sentences showing the relationships.

**Example:** "One member can have many loans." "One book can be loaned many times (but only one at a time)."

_______________________________________________
_______________________________________________
_______________________________________________

---

### Exercise 1.7: Database Type Matchmaker

**Task:** For each scenario below, decide which **type of database** would be most appropriate. Choose from: Relational, NoSQL, Key-Value, Graph, or Time-Series.

| Scenario | Best Database Type | Why? |
|----------|-------------------|------|
| A bank managing millions of accounts with strict rules about balances and transactions | | |
| A social media app storing billions of user posts with varying formats (text, photos, videos) | | |
| A stock trading platform recording price changes every millisecond | | |
| A fraud detection system analyzing who knows whom in a criminal network | | |
| A website storing user login sessions for fast lookup | | |
| A university managing students, courses, grades, and departments with clear relationships | | |

---

### Exercise 1.8: The Pizza Shop Problem

**Task:** A small pizza shop wants to start taking online orders. The owner asks you: "Can't I just use a bunch of text files to track orders?"

**Your task:** Write a short, friendly response (3-5 sentences) explaining why a database would be better. Use concepts from this module. Don't use jargon — explain it like you're talking to the shop owner, not a tech conference.

**Your response:**

_______________________________________________
_______________________________________________
_______________________________________________
_______________________________________________
_______________________________________________

---

## 🔴 Challenging Exercises

These exercises stretch your thinking. They're not about memorization — they're about reasoning, creativity, and problem-solving.

---

### Exercise 1.9: The University Management System — Deep Dive

**Task:** We're going to use the University Management System throughout this entire course. Let's start thinking about it deeply now.

Our six core tables are:
1. **students**
2. **instructors**
3. **departments**
4. **courses**
5. **enrollments**
6. **grades**

**Part A:** For each table, brainstorm at least **6 columns** (pieces of information) you think should be stored.

**students**

| # | Column | Example Value | Why Important? |
|---|--------|---------------|----------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

**instructors**

| # | Column | Example Value | Why Important? |
|---|--------|---------------|----------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

**departments**

| # | Column | Example Value | Why Important? |
|---|--------|---------------|----------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

**courses**

| # | Column | Example Value | Why Important? |
|---|--------|---------------|----------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

**enrollments**

| # | Column | Example Value | Why Important? |
|---|--------|---------------|----------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

**grades**

| # | Column | Example Value | Why Important? |
|---|--------|---------------|----------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |

**Part B:** Think about the relationships. Answer these questions:

1. Can one department have many instructors? _______
2. Can one instructor teach many courses? _______
3. Can one student have many enrollments? _______
4. Can one enrollment have many grades? _______
5. Can one course have many enrollments? _______

**Part C:** Draw a simple diagram showing how these tables connect. Use arrows and labels like "has many" or "belongs to."
