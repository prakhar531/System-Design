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

- **Step 1:** Define the scope - Ask questions (5-10 minutes):
  Talk about problem. No tech discussion. User focused target questions.
- **Step 2:** Design at a high-level (5-10 minutes):
  Lay down blueprint of it. Basic design/framework.Pick components at high level. No details.
- **Step 3:** Deep-dive in 1-2 components (10-15 minutes)
  define edge cases
- **Step 4:** Identify points of failure / bottlenecks / scalability issues and fix the issues (10-15 minutes)
- **Step 5:** Summarize, extend, and discuss alternatives (5-10 minutes)

# Database

A database is an organized collection of data stored in a computer system and usually controlled by a database management system (DBMS). The data in common databases is modeled in tables, making querying and processing efficient. Structured query language (SQL) is commonly used for data querying and writing.

The Database is an essential part of our life. We encounter several activities that involve our interaction with databases, for example in the bank, in the railway station, in school, in a grocery store, etc. These are the instances where we need to store a large amount of data in one place and fetch these data easily.

## Types of Databases

1. **SQL**
2. **NoSQL**

### Main differences between NoSQL and SQL

### **Type**

[SQL](https://www.geeksforgeeks.org/sql-tutorial/) databases are primarily called Relational Databases [(RDBMS)](https://www.geeksforgeeks.org/rdbms-full-form/); whereas [NoSQL databases](https://www.geeksforgeeks.org/introduction-to-nosql/) are primarily called non-relational or distributed databases.

### **Language** 

SQL databases define and manipulate data-based [structured query language (SQL)](https://www.geeksforgeeks.org/structured-query-language/). Seeing from a side this language is extremely powerful. SQL is one of the most versatile and widely-used options available which makes it a safe choice, especially for great complex queries. But from another side, it can be restrictive. SQL requires you to use predefined [schemas](https://www.geeksforgeeks.org/create-schema-in-sql-server/) to determine the structure of your data before you work with it. Also, all of your data must follow the same structure. This can require significant up-front preparation which means that a change in the structure would be both difficult and disruptive to your whole system.

A NoSQL database has a dynamic schema for unstructured data. Data is stored in many ways which means it can be document-oriented, column-oriented, graph-based, or organized as a key-value store. This flexibility means that documents can be created without having a defined structure first. Also, each document can have its own unique structure. The syntax varies from database to database, and you can add fields as you go.

### **Scalability**

In almost all situations SQL databases are vertically scalable. This means that you can increase the load on a single server by increasing things like [RAM](https://www.geeksforgeeks.org/random-access-memory-ram/), [CPU,](https://www.geeksforgeeks.org/central-processing-unit-cpu/) or [SSD](https://www.geeksforgeeks.org/introduction-to-solid-state-drive-ssd/). But on the other hand, NoSQL databases are horizontally scalable. This means that you handle more traffic by sharing, or adding more servers in your NoSQL database. It is similar to adding more floors to the same building versus adding more buildings to the neighborhood. Thus NoSQL can ultimately become larger and more powerful, making these databases the preferred choice for large or ever-changing data sets.

### **Structure** 

SQL databases are table-based on the other hand NoSQL databases are either key-value pairs, document-based, graph databases, or wide-column stores. This makes relational SQL databases a better option for applications that require multi-row transactions such as an accounting system or for legacy systems that were built for a relational structure.

Here is a simple example of how a structured data with rows and columns vs a non-structured data without definition might look like. A product table in SQL dbmight accept data looking like this:

SQL`{ "id": "101", "category":"food" "name":"Apples", "qty":"150" }`

Whereas a unstructured NOSQL DB might save the products in many variations without constraints to change the underlying table structure

NoSQL`Products=[ { "id":"101: "category":"food",, "name":"California Apples", "qty":"150" }, { "id":"102, "category":"electronics" "name":"Apple MacBook Air", "qty":"10", "specifications":{    "storage":"256GB SSD",    "cpu":"8 Core",    "camera": "1080p FaceTime HD camera"   } } ]`

### **Property followed**

SQL databases follow [ACID properties](https://www.geeksforgeeks.org/acid-properties-in-dbms/) (Atomicity, Consistency, Isolation, and Durability) whereas the NoSQL database follows the Brewers [CAP theorem](https://www.geeksforgeeks.org/the-cap-theorem-in-dbms/) (Consistency, Availability, and Partition tolerance).

### **Support**

Great support is available for all SQL databases from their vendors. Also, a lot of independent consultants are there who can help you with SQL databases for very large-scale deployments but for some NoSQL databases you still have to rely on community support and only limited outside experts are available for setting up and deploying your large-scale NoSQL deploy.

## When To Use: SQL vs NoSQL 

SQL is a good choice when working with related data. Relational databases are efficient, flexible, and easily accessed by any application. A benefit of a relational database is that when one user updates a specific record, every instance of the database automatically refreshes, and that information is provided in real-time.

SQL and a relational database make it easy to handle a great deal of information, scale as necessary and allow flexible access to data only needing to update data once instead of changing multiple files, for instance. It’s also best for assessing data integrity. Since each piece of information is stored in a single place, there’s no problem with former versions confusing the picture.

While NoSQL is good when the availability of big data is more crucial, SQL is valued for ensuring data validity. It’s also a wise decision when a business needs to expand in response to shifting customer demands. NoSQL offers high performance, flexibility, and ease of use.

NoSQL is also a wise choice when dealing with large or constantly changing data sets, flexible data models, or requirements that don’t fit into a relational model. Document databases, like CouchDB, MongoDB, and Amazon DocumentDB, are useful for handling large amounts of unstructured data.

## **Key Highlights on SQL vs NoSQL**

| SQL                                                                                                                                                                                        | NoSQL                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| RELATIONAL DATABASE MANAGEMENT SYSTEM (RDBMS)                                                                                                                                              | Non-relational or distributed database system.                                                         |
| These databases have fixed or static or predefined schema                                                                                                                                  | They have a dynamic schema                                                                             |
| These databases are not suited for hierarchical data storage.                                                                                                                              | These databases are best suited for hierarchical data storage.                                         |
| These databases are best suited for complex queries                                                                                                                                        | These databases are not so good for complex queries                                                    |
| Vertically Scalable                                                                                                                                                                        | Horizontally scalable                                                                                  |
| Follows ACID property                                                                                                                                                                      | Follows CAP(consistency, availability, partition tolerance)                                            |
| **Examples:** [MySQL](https://www.geeksforgeeks.org/mysql-common-mysql-queries/), [PostgreSQL](https://www.geeksforgeeks.org/what-is-postgresql-introduction/), Oracle, MS-SQL Server, etc | **Examples:** [MongoDB](https://www.geeksforgeeks.org/mongodb-tutorial/), HBase, Neo4j, Cassandra, etc |

### Examples of SQL: MySQL, Postgres, etc

### ACID Properties

# Transactions and ACID Properties

A [**transaction**](https://www.geeksforgeeks.org/sql-transactions) is a single logical unit of work that accesses and possibly modifies the contents of a database. Transactions access data using read and write operations. In order to maintain consistency in a database, before and after the transaction, certain properties are followed. These are called **ACID** properties.

![ACID Properties](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20191121102921/ACID-Properties.jpg)

For those looking to master these concepts and excel in exams like **GATE**, our [**GATE course**](https://gfgcdn.com/tu/Q1a/) offers an in-depth exploration of database management systems. We cover everything from the basics to advanced topics, ensuring a thorough understanding that is essential for high scores and practical application.

### **Atomicity**

By this, we mean that either the entire transaction takes place at once or doesn’t happen at all. There is no midway, i.e., transactions do not occur partially. Each transaction is considered one unit and either runs to completion or is not executed at all. It involves the following two operations:

- **Abort**: If a transaction aborts, changes made to the database are not visible.
- **Commit**: If a transaction commits, changes made are visible.

Atomicity is also known as the ‘All or nothing rule’.

Consider the following transaction **T** consisting of **T1** and **T2**: Transfer of 100 from account **X** to account **Y**.

![Transaction Example](https://media.geeksforgeeks.org/wp-content/uploads/11-6.jpg)

If the transaction fails after the completion of **T1** but before the completion of **T2** (say, after **write(X)** but before **write(Y)**), then the amount has been deducted from **X** but not added to **Y**. This results in an inconsistent database state. Therefore, the transaction must be executed in its entirety to ensure the correctness of the database state.

### **Consistency**

This means that integrity constraints must be maintained so that the database is consistent before and after the transaction. It refers to the correctness of a database. Referring to the example above, the total amount before and after the transaction must be maintained.  
Total **before T** occurs = **500 + 200 = 700**.  
Total **after T occurs** = **400 + 300 = 700**.  
Therefore, the database is **consistent**. Inconsistency occurs if **T1** completes but **T2** fails. As a result, T is incomplete.

### **Isolation**

This property ensures that multiple transactions can occur concurrently without leading to the inconsistency of the database state. Transactions occur independently without interference. Changes occurring in a particular transaction will not be visible to any other transaction until that particular change in that transaction is written to memory or has been committed. This property ensures that the execution of transactions concurrently will result in a state that is equivalent to a state achieved if these were executed serially in some order.

Let **X = 500**, **Y = 500**.  
Consider two transactions **T** and **T'**.

![Isolation Example](https://media.geeksforgeeks.org/wp-content/uploads/20210402015259/isolation-300x137.jpg)

Suppose **T** has been executed till **Read (Y)** and then **T'** starts. As a result, interleaving of operations takes place due to which **T'** reads the correct value of **X** but the incorrect value of **Y** and the sum computed by **T'**:  
**(X+Y = 500 + 500 = 1000)**  
is thus not consistent with the sum at the end of the transaction:  
**T: (X+Y = 500 + 450 = 950)**.  
This results in database inconsistency due to a loss of 50 units. Hence, transactions must take place in isolation and changes should be visible only after they have been made to the main memory.

### **Durability**

This property ensures that once the transaction has completed execution, the updates and modifications to the database are stored in and written to disk, and they persist even if a system failure occurs. These updates now become permanent and are stored in non-volatile memory. The effects of the transaction, thus, are never lost.

### **Some Important Points:**

| **Property** | **Responsibility for Maintaining Properties** |
| ------------ | --------------------------------------------- |
| Atomicity    | Transaction Manager                           |
| Consistency  | Application Programmer                        |
| Isolation    | Concurrency Control Manager                   |
| Durability   | Recovery Manager                              |

The **ACID** properties, in totality, provide a mechanism to ensure the correctness and consistency of a database in a way such that each transaction is a group of operations that acts as a single unit, produces consistent results, acts in isolation from other operations, and updates that it makes are durably stored.

ACID properties are the four key characteristics that define the reliability and consistency of a transaction in a Database Management System (DBMS). The acronym ACID stands for Atomicity, Consistency, Isolation, and Durability. Here is a brief description of each of these properties:

1. **Atomicity**: Ensures that a transaction is treated as a single, indivisible unit of work. Either all the operations within the transaction are completed successfully, or none of them are. If any part of the transaction fails, the entire transaction is rolled back to its original state, ensuring data consistency and integrity.
2. **Consistency**: Ensures that a transaction takes the database from one consistent state to another consistent state. The database is in a consistent state both before and after the transaction is executed. Constraints, such as unique keys and foreign keys, must be maintained to ensure data consistency.
3. **Isolation**: Ensures that multiple transactions can execute concurrently without interfering with each other. Each transaction must be isolated from other transactions until it is completed. This isolation prevents dirty reads, non-repeatable reads, and phantom reads.
4. **Durability**: Ensures that once a transaction is committed, its changes are permanent and will survive any subsequent system failures. The transaction’s changes are saved to the database permanently, and even if the system crashes, the changes remain intact and can be recovered.

Overall, ACID properties provide a framework for ensuring data consistency, integrity, and reliability in DBMS. They ensure that transactions are executed in a reliable and consistent manner, even in the presence of system failures, network issues, or other problems. These properties make DBMS a reliable and efficient tool for managing data in modern organizations.

### **Advantages of ACID Properties in DBMS:**

1. **Data Consistency**: ACID properties ensure that the data remains consistent and accurate after any transaction execution.
2. **Data Integrity**: ACID properties maintain the integrity of the data by ensuring that any changes to the database are permanent and cannot be lost.
3. **Concurrency Control**: ACID properties help to manage multiple transactions occurring concurrently by preventing interference between them.
4. **Recovery**: ACID properties ensure that in case of any failure or crash, the system can recover the data up to the point of failure or crash.

### **Disadvantages of ACID Properties in DBMS:**

1. **Performance**: The ACID properties can cause a performance overhead in the system, as they require additional processing to ensure data consistency and integrity.
2. **Scalability**: The ACID properties may cause scalability issues in large distributed systems where multiple transactions occur concurrently.
3. **Complexity**: Implementing the ACID properties can increase the complexity of the system and require significant expertise and resources.

Overall, the advantages of ACID properties in DBMS outweigh the disadvantages. They provide a reliable and consistent approach to data management, ensuring data integrity, accuracy, and reliability. However, in some cases, the overhead of implementing ACID properties can cause performance and scalability issues. Therefore, it’s important to balance the benefits of ACID properties against the specific needs and requirements of the system.

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
