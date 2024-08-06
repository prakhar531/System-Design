# Building Blocks of System Design

## Topics Covered

1. **Introduction to System Design (SD)**
   - What happens in System Design?
   - Why is it considered tough?
   - How should we approach System Design questions?
2. **System Design Framework and Timelines**
3. **Types of Databases (DB)**

## Introduction to System Design

### What happens in System Design?

1. **Vague Questions**
   - Ask clarifying questions
   - Follow a framework
   - Time-bound yourself
   - Stay on track

### Why is System Design considered tough?

1. **No "Right" Answer!**
   - No good or bad design
   - Justifying our choices
   - Explain/call out trade-offs
     - Highlight pros and cons
     - Why do pros outweigh cons?

### How should we approach System Design questions?

1. **Two-way conversation**
   - Think out loud
   - Check-in with your teammate
   - Summarize your design
   - Check on where to dive deeper

## System Design Framework and Timelines

- **Step 1:** Define the scope - Ask questions (5-10 minutes)
- **Step 2:** Design at a high-level (5-10 minutes)
- **Step 3:** Deep-dive in 1-2 components (10-15 minutes)
- **Step 4:** Identify points of failure / bottlenecks / scalability issues and fix the issues (10-15 minutes)
- **Step 5:** Summarize, extend, and discuss alternatives (5-10 minutes)

## Types of Databases

1. **SQL**
2. **NoSQL**

### Examples of SQL: MySQL, Postgres, etc

### ACID Properties

- **A ⇒ Atomicity**
  - All statements in a transaction will either be fully completed or fully rolled back in case of failure.
- **C ⇒ Consistency**
  - Data across tables will always be consistent.
  - Deleting primary keys without deleting foreign keys is not permitted.
- **I ⇒ Isolation**
  - One transaction will not interfere with the changes being done in another transaction.
  - Isolation ensures data integrity.
- **D ⇒ Durability**
  - Do not accidentally delete data ever.
  - If your DB crashes, your data should not be lost.
  - A transaction that has been committed should always persist in DB, even in case of system crashes.

### Transaction ⇒ Group of statements that is executed together

```sql
BEGIN TRANSACTION;
// Deduct money from my account
UPDATE account_balance
SET account_balance = account_balance - 100
WHERE account_owner = "Abhishek";

// Add money in your account
UPDATE account_balance
SET account_balance = account_balance + 100
WHERE account_owner = "student1";

// DB Crashes here
COMMIT;
```
