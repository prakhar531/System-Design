# What is **Isolation Level**?

Isolation in database refers to the ability of a database system to allow multiple transactions to access the same data without interfering with each other. Isolation ensures that each transaction sees a consistent view of the data, regardless of the concurrent activities of other transactions.
As we know, to maintain consistency in a database, it follows **ACID** properties. Among these four properties (Atomicity, Consistency, Isolation, and Durability) Isolation means that a transaction should take place in a system in such a way that it is the only transaction that is accessing the resources in a database system.

# Why do we need isolation?

Imagine that you’re implementing a system for large e-commerce. Many operations have to take place at the same time, multiple customers may simultaneously want to purchase the same product, prices of some product may change, new products are still being delivered, etc. As you know, a single action done by a user is run as a transaction in a database, so we need to add some logic to maintain consistency and that’s the role of Isolation because it controls:

- Whether locks are taken when data is read, and what type of locks are requested.
- How long the read locks are held.
- Whether a read operation referencing rows modified by another transaction so it Blocks until the exclusive lock on the row is freed or Retrieves the committed version of the row that existed at the time the statement or transaction started it’s depending on the isolation levels

# What is an “Isolation Level”?

**Database isolation** defines the degree to which a transaction must be isolated from the data modifications made by any other transaction(even though in reality there can be a large number of concurrently running transactions). The overarching goal is to prevent reads and writes of temporary, aborted, or otherwise incorrect data written by concurrent transactions.

# Transaction isolation level is defined by the following phenomena

## Dirty Reads

A transaction reads data written by a concurrent uncommitted transaction. (uncommitted data is called “dirty.”)
Dirty reads occur when a transaction reads data that has been modified by another transaction but not yet committed. This means that the transaction is reading data that is still in an intermediate or "dirty" state, and it may be rolled back later.

![](https://miro.medium.com/v2/resize:fit:379/1*3Rb475q3vk7UMGFW4rB4IQ.png)

For example, Let’s say transaction 1 updates a row and leaves it uncommitted, meanwhile, Transaction 2 reads the updated row. If transaction 1 rolls back the change, transaction 2 will have read data that is considered never to have existed.

To illustrate this, let's consider a simple example. Suppose we have a table named "employees" with the following columns: "id", "name", "salary", and "department". Transaction A executes the following query:

SELECT salary FROM employees WHERE id = 1;

Meanwhile, Transaction B updates the salary of employee with id 1 using the following query:

UPDATE employees SET salary = 60000 WHERE id = 1;

However, Transaction B does not commit the update yet. If we are using Read Uncommitted isolation level, Transaction A can read the new value of the salary column even though Transaction B has not committed the update yet.

To demonstrate this, let's run the following SQL queries:

```sql
\-- Start Transaction A
BEGIN TRANSACTION;
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT salary FROM employees WHERE id = 1;
-- End Transaction A

-- Start Transaction B
BEGIN TRANSACTION;
UPDATE employees SET salary = 60000 WHERE id = 1;
-- End Transaction B

```

If we are using Read Uncommitted isolation level, the read operation in Transaction A may return the new value of the salary column even though Transaction B has not committed the update yet.

To prevent dirty reads, we can use at least the Read Committed isolation level, which ensures that a transaction reads only committed data.

In SQL, we can set the isolation level for a transaction using the SET TRANSACTION statement. To set the isolation level to Read Committed, we would use the following query:

SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

In conclusion, dirty reads can occur in database systems when a transaction reads data that has been modified by another transaction but not yet committed. To prevent dirty reads, we can use at least the Read Committed isolation level, which ensures that a transaction reads only committed data.

## Non-Repeatable Reads, and Read Skew

A transaction re-reads data it has previously read and finds that data has been modified by another transaction (that has been committed since the initial read). Non-repeatable reads occur when a transaction reads a row twice but gets different values in each read. This can happen when another transaction modifies the row in between the two reads.

Note that this differs from a dirty read in that the other transaction has been committed. Also, this phenomenon requires two reads to manifest.

![img](https://miro.medium.com/v2/resize:fit:416/1*NRQAi5Y4GYYJ_uXmsHhPpA.png)

For example, suppose transaction T1 reads data. Due to concurrency, another transaction T2 updates the same data and commit, Now if transaction T1 rereads the same data, it will retrieve a different value

To demonstrate this, let's run the following SQL queries:

```sql
\-- Start Transaction A
BEGIN TRANSACTION;
SELECT salary FROM employees WHERE id = 1;
-- End Transaction A

-- Start Transaction B
BEGIN TRANSACTION;
UPDATE employees SET salary = 60000 WHERE id = 1;
COMMIT;
-- End Transaction B

-- Start Transaction A again
BEGIN TRANSACTION;
SELECT salary FROM employees WHERE id = 1;
-- End Transaction A again
```

If we are using Repeatable Read isolation level, the second read operation in Transaction A may return a different value for the salary column compared to the first read operation, since the row was updated by Transaction B.

To prevent non-repeatable reads, we can use Serializable isolation level, which ensures that a transaction sees a consistent snapshot of the database throughout its execution, even if other transactions modify the same data.

In SQL, we can set the isolation level for a transaction using the SET TRANSACTION statement. To set the isolation level to Serializable, we would use the following query:

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

In conclusion, non-repeatable reads can occur in database systems when a transaction reads a row twice but gets different values in each read, due to another transaction modifying the row in between. To prevent non-repeatable reads, we can use Serializable isolation level, which ensures that a transaction sees a consistent snapshot of the database throughout its execution.

## Phantom Reads

Phantom reads refer to the situation where a transaction reads a set of rows that satisfy a certain condition, but when the same transaction repeats the same read operation later, additional rows appear that were not visible before. This happens when another transaction commits a new row that satisfies the same condition.

This is similar to a non-repeatable read except it involves a changing collection matching a predicate rather than a single item.

![](https://miro.medium.com/v2/resize:fit:365/1*R1pXxAd0PQu0bzHe2YPgAQ.png)

For example, suppose transaction T1 retrieves a set of rows that satisfy some search criteria. Now, Transaction T2 generates some new rows that match the search criteria for transaction T1. If transaction T1 re-executes the statement that reads the rows, it gets a different set of rows this time.

o demonstrate this, let's run the following SQL queries:

```sql
\-- Start Transaction A
BEGIN TRANSACTION;
SELECT \* FROM employees WHERE department = 'Sales';
-- End Transaction A

-- Start Transaction B
BEGIN TRANSACTION;
INSERT INTO employees (name, salary, department) VALUES ('John Doe', 50000, 'Sales');
COMMIT;
-- End Transaction B

-- Start Transaction A again
BEGIN TRANSACTION;
SELECT \* FROM employees WHERE department = 'Sales';
-- End Transaction A again
```

If we are using Repeatable Read isolation level, the second read operation in Transaction A may return a new row inserted by Transaction B. This is because Repeatable Read isolation level uses a snapshot of the database taken at the beginning of the transaction, and any subsequent changes made by other transactions are not visible to the current transaction.

To prevent phantom reads, we can use **Serializable isolation** level, which ensures that a transaction sees a consistent snapshot of the database throughout its execution, even if other transactions modify the same data.

In SQL, we can set the isolation level for a transaction using the SET TRANSACTION statement. To set the isolation level to Serializable, we would use the following query:

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

In conclusion, phantom reads can occur in database systems when a transaction reads a set of rows that satisfy a certain condition, but additional rows appear when the same transaction repeats the same read operation later. To prevent phantom reads, we can use Serializable isolation level, which ensures that a transaction sees a consistent snapshot of the database throughout its execution.

## Write Skew

Two concurrent transactions each determine what they are writing based on reading a data set that overlaps what the other is writing.

![](https://miro.medium.com/v2/resize:fit:514/1*P8iNcRGHzc267FaQC_jXVw.png)

For example, suppose 2 transactions read that x and y have value 100, so individually it’s fine for each transaction to negate one of the values, the total would still be non-negative. However negating both values results in x+y=-200, violating the constraint. For emotional gravity, this is usually framed in terms of bank accounts where account balances are allowed to go negative as long as the sum of commonly held balances remains non-negative.

# Based on these phenomena, These four isolation levels have been defined

Database Isolation Levels:

1.  Read Uncommitted- Level 0
2.  Read Committed- Level 1
3.  Repeatable Read- Level 2 ⟹ Default
4.  Serializable- Level 3 ⟹ Strictest

## **Read Uncommitted**

![](https://miro.medium.com/v2/resize:fit:423/1*r-alkyoITFPb1OJG9W3AeA.png)

Read Uncommitted is the lowest isolation level. At this level, makes sure **no transaction can update a database row if another transaction has already updated it and not committed**. This protects against lost updates, but won’t stand in a way of dirty reads.

Read uncommitted is a type of db isolation where we are reading data that is uncommitted by other user. It is risky.
Best example: where only being read. Such as distributed logs.
Log file don not change state of system it just pass comments.
At any stage it will read uncommitted values.

## **Read Committed** : (commit based isolation level)

![](https://miro.medium.com/v2/resize:fit:423/1*R8zg5BQVwrF8_VHUOKDiaA.png)

This isolation level **does not allow any other transaction to write or read a row to which another transaction has written to but not yet committed**. Thus it does not allows dirty read. The transaction holds a read or write lock on the current row, and thus prevents other transactions from reading, updating, or deleting it.
Until user commit changes it will not reflect changes to other user. Means read only committed data.
Best example: Updating stock levels, preventing to buy negative stocks

## **Repeatable Read** : (transaction based isolation level)

![](https://miro.medium.com/v2/resize:fit:411/1*cIyttWsKt2sx90JavM3WUw.png)

This isolation level makes sure any **transaction that reads data from a row blocks any other writing transactions from accessing the same row.** This is the most restrictive isolation level that holds read locks on all rows it references and writes locks on all rows it inserts, updates, or deletes. Since other transactions cannot read, update or delete these rows, consequently it avoids non-repeatable read.

As a transaction is not committed that session will get same value as start of transaction. i.e. values will remains same throughout transaction until it commit or end transaction.
Main point is whenever it read from database it reads most recent/committed value . First read will be most recent committed value after that it will read repeated value
Use case: booking system.

## **Serializable**

![](https://miro.medium.com/v2/resize:fit:488/1*bPZH7erIMV4J14kfLlL0-A.png)

This isolation level is the highest isolation level. serializable isolation level requires a lot more than restricting access to a single row. Typically this isolation mode would **lock the whole table**, to prevent any other transactions from inserting or reading data from it.

serializable execution is defined to be an execution of operations in which concurrently executing transactions appears to be serially executing.

We cannot have two wites at same time. It will wait for first transaction to finish then it will update next one. It starts with first update or delete or insert.
Use case: Any financial system.

## Snapshot Isolation

![](https://miro.medium.com/v2/resize:fit:490/1*VZuYyGkbBDcbSuwRmuufEw.png)

This isolation level can greatly increase concurrency at a lower cost than transactional isolation. When data is modified, the committed versions of affected rows are copied to temp and given version numbers. This operation is called copy on write and is used for all inserts, updates, and deletes using this technique. When another session reads the same data, the committed version of the data as of the time the reading transaction began is returned.

## SQL Scripts:

```sql
 Unset
 USE algoprep;
 SELECT @@transaction_ISOLATION;
 SET autocommit=0;

-- Repeatable read
 START TRANSACTION;
 SELECT * FROM persons where id=1;
 UPDATE persons SET fname = "abhi_t1" WHERE id = 1;
 SELECT * FROM persons where id=1;

-- moving to read committed
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
 SELECT @@transaction_ISOLATION;
 START TRANSACTION;
 SELECT * FROM persons where id=1;
 UPDATE persons SET fname = "abhi_read_committed" WHERE id = 1;
 SELECT * FROM persons where id=1;

-- moving to read uncommitted
 SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
 SELECT @@transaction_ISOLATION;
 START TRANSACTION;
 SELECT * FROM persons where id=1;
 UPDATE persons SET fname = "abhi_uncommitted_t1" WHERE id = 1;
 SELECT * FROM persons where id=1;

-- moving to serializable
 SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
 SELECT @@transaction_ISOLATION;
 START TRANSACTION;
 SELECT * FROM persons where id=1;
 UPDATE persons SET fname = "abhi_serializable_t1" WHERE id = 1;
 SELECT * FROM persons where id=1;

```

## Real Life Examples:

### 1. Read Uncommitted:

- **Use Case:**
  - Rarely used in practice due to its minimal isolation guarantees.
- **Example:**
  - In a high-throughput logging system where immediate visibility of log entries is essential, you might use this isolation level. Even if a transaction hasn't been committed, other transactions can read the uncommitted data.

### 2. Read Committed:

- **Use Case:**
  - Used in most transactional systems where some level of isolation is needed.
- **Example:**
  - In an e-commerce platform, when updating product stock levels, you want to prevent users from seeing negative stock values or purchasing items that are out of stock. Read Committed ensures that only committed changes are visible to other transactions.

### 3. Repeatable Read:

- **Use Case:**
  - Useful when you need to maintain a consistent snapshot of data during a transaction.
- **Example:**
  - In a reservation system for a hotel, you don't want a customer to book a room that has become unavailable since they first viewed the available rooms. Repeatable Read ensures that the set of rooms a customer sees during their booking process remains constant.

### 4. Serializable:

- **Use Case:**
  - When strict isolation is required, often used in financial systems.
- **Example:**
  - In a banking application, you want to ensure that two transactions, such as transferring money between accounts, don't interfere with each other. Serializable isolation ensures that transactions occur as if they were executed one after the other, preventing conflicts.

### 5. Snapshot Isolation:

- **Use Case:**
  - Useful when you need to provide consistent views of data without locking. MySQL does not support Snapshot Isolation.
- **Example:**
  - In a content management system, multiple authors may be editing articles simultaneously. Snapshot Isolation allows each author to work on their copy of an article without locking it, and changes are merged when saving without blocking others.

### 6. No Isolation:

- **Use Case:**
  - Rarely used in real-world scenarios, as it provides no isolation between transactions.
- **Example:**
  - In a single-user application where there's no concurrent access or where data consistency isn't critical, you might choose not to enforce any isolation level. However, this is uncommon in multi-user database systems.

# Which transaction isolation to choose

![](https://miro.medium.com/v2/resize:fit:700/1*W1YGmFT6j4GTVndNUzA7pA.jpeg)

The choice of transaction isolation level depends on the details of each specific case. These hints may be helpful, but please consider each situation individually.

When designing your application, you definitely want to ensure that none of your database transactions read uncommitted changes of other transactions**.**

because changes can easily harm your data integrity, as reverted changes in one transaction can be read and potentially accepted by another. The minimum isolation level to ensure in your application, therefore, is Read Committed.  
Most of the time you probably don’t need Serializable isolation, as it can cause big performance issues with a large volume of concurrent transactions.  
so it’s always depends on your business requirements
