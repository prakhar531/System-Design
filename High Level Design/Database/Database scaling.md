# Scaling

\***\*Scaling\*\*** alters the size of a system. In the scaling process, we either compress or expand the system to meet the expected needs. The scaling operation can be achieved by adding resources to meet the smaller expectation in the current system, by adding a new system to the existing one, or both.

## Types of Scaling: 

![Horizontal-and-Vertical-Scaling-In-Databases-2-copy](https://media.geeksforgeeks.org/wp-content/uploads/20240616171706/Horizontal-and-Vertical-Scaling-In-Databases-2-copy.webp)

Scaling can be categorized into 2 types:

1. \***\*Vertical Scaling:\*\*** When new resources are added to the existing system to meet the expectation, it is known as vertical scaling.

   Consider a rack of servers and resources that comprises the existing system. (as shown in the figure). Now when the existing system fails to meet the expected needs, and the expected needs can be met by just adding resources, this is considered vertical scaling. Vertical scaling is based on the idea of adding more power(CPU, RAM) to existing systems, basically adding more resources.  
   Vertical scaling is not only easy but also cheaper than Horizontal Scaling. It also requires less time to be fixed.

2. \***\*Horizontal Scaling:\*\*** When new server racks are added to the existing system to meet the higher expectation, it is known as horizontal scaling.

Consider a rack of servers and resources that comprises the existing system. (as shown in the figure). Now when the existing system fails to meet the expected needs, and the expected needs cannot be met by just adding resources, we need to add completely new servers. This is considered horizontal scaling. Horizontal scaling is based on the idea of adding more machines to our pool of resources. Horizontal scaling is difficult and also costlier than Vertical Scaling. It also requires more time to be fixed.

### Differences between Horizontal and Vertical Scaling are as follows:

| Horizontal Scaling                                                                                                        | Vertical Scaling                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| When new server racks are added to the existing system to meet the higher expectation, it is known as horizontal scaling. | When new resources are added in the existing system to meet the expectation, it is known as vertical scaling |
| It expands the size of the existing system horizontally.                                                                  | It expands the size of the existing system vertically.                                                       |
| It is easier to upgrade.                                                                                                  | It is harder to upgrade and may involve downtime.                                                            |
| It is difficult to implement                                                                                              | It is easy to implement                                                                                      |
| It is costlier, as new server racks comprise a lot of resources                                                           | It is cheaper as we need to just add new resources                                                           |
| It takes more time to be done                                                                                             | It takes less time to be done                                                                                |
| High resilience and fault tolerance                                                                                       | Single point of failure                                                                                      |
| Examples of databases that can be easily scaled- Cassandra, MongoDB, Google Cloud Spanner                                 | Examples  of databases that can be easily scaled- MySQL, Amazon RDS                                          |

# Database Replication

Database replication refers to the process of copying data from a primary database to one or more replica databases in order to improve data accessibility and system fault-tolerance and reliability.

## What is synchronous replication?

If you think about a distributed database system, the obvious way to prevent data loss is to ensure that data isn’t committed to any node unless it’s been committed to all nodes. This is called synchronous replication.

In a two-node system with synchronous replication, for example, data isn’t fully committed to either node until it has been sent to the replica node and the replica node has send confirmation back to the primary node that it was received:

[![synchronous-replication-database-data-loss](https://images.ctfassets.net/00voh0j35590/bVTT5dNbpSQAY9YVmq97w/4f2196ac178b957d8fdc993ec7025208/synchronous-replication-database-data-loss.jpg)](https://images.ctfassets.net/00voh0j35590/bVTT5dNbpSQAY9YVmq97w/4f2196ac178b957d8fdc993ec7025208/synchronous-replication-database-data-loss.jpg)

This approach works well to ensure consistency between the nodes. If the primary node were to go offline, no data would be lost, as any data committed to the primary node has also been committed to the replica node.

However, synchronous replication introduces latency into the system, since the primary node has to send the write and then wait for confirmation from the replica node before it can commit.

This additional latency will be minimal if there are only two nodes and they’re deployed relatively close to each other (for example, in the same cloud AZ). But for most production workloads, and especially mission-critical workloads, there are likely to be more than two nodes, and these nodes may be spread across different AZs or different cloud regions to ensure the system is highly available. Each additional node and region increases the latency, since there are more steps required and the data has to travel farther before a write can be fully committed.

Synchronous replication provides robust data protection, but at the cost of very high write latency. This very high write latency can be crippling for many applications, so they look to asynchronous or semi-synchronous solutions, and that’s where the risk of data loss can creep in.

## What is asynchronous replication?

In asynchronous replication, you have a “primary” database node that is the only one that accepts writes from the application. When writes are the primary node acknowledges the committed write to the application, and the application moves on assuming that write is permanent. Then the write is synced to a replica node (or nodes), creating an exact copy of the primary node. In the event the primary node goes down, you fail over to a replica node, making that the new primary.

Here’s an illustration of how it works with a single replica node:

[![asynchronous-replication-database-data-loss](https://images.ctfassets.net/00voh0j35590/1GS6u3uQ4pWZIRnrniBmnF/3dc7e78bd2055310cce786089a27a9a5/asynchronous-replication-database-data-loss.jpg)](https://images.ctfassets.net/00voh0j35590/1GS6u3uQ4pWZIRnrniBmnF/3dc7e78bd2055310cce786089a27a9a5/asynchronous-replication-database-data-loss.jpg)

This type of system is called asynchronous replication because the data replication happens _after_ the write has been committed on the primary node. And that’s where the potential for data loss comes in.

### How you can lose data with asynchronous replication

Imagine a write comes in, is committed to the primary node, the application receives the acknowledgement that the write succeeded, and then the primary node immediately goes offline. Because the replication is not synchronous, the write wasn’t sent to the replica node before the primary node went down. Thus, when the system fails over to the replica node, that write – and any subsequent writes that have come in while the failover is happening – is lost. It’s still stored on that primary node machine, but by the time that comes online the other node will have committed additional writes, at which point there’s no way to merge back in that initial lost write.

![alt text](<./assets/Screenshot (189).png>)

![alt text](<./assets/Screenshot (190).png>)

## What is semi-synchronous replication?

Semi-synchronous replication is essentially the same as synchronous replication, except that a semi-synchronous system will revert to asynchronous replication if the latency between nodes passes a certain threshold.

This removes the “latency penalty” associated with fully synchronous replication, but it also means that anytime the system is stressed, it is likely to revert to asynchronous replication, meaning that it can lose data in the exact same manner as an asynchronous system.

From a data loss perspective, a semi-synchronous system is really the worst of both worlds: you get a lot of the additional latency associated with synchronous replication, but you’re still vulnerable to data loss if the system gets stressed and then a node goes offline.

# Replication Strategies

There are 3 types of replication strategies

1. Leader- Follower Replication
2. Multi-Leader Replication
3. Leaderless Replication

## Leader- Follower Replication

- Primary node is responsible for handling all write requests
- Follower nodes are used for handling read requests
- Appropriate when our workload is read-heavy
- For write-heavy applications, leader-follower replication is not a good idea ever
- We can scale for readability, by adding more read replicas
  Follower nodes can handle read requests even when the leader is down

![alt text](<./assets/Screenshot (192).png>)

## Multi-Leader Replication

- There are multiple primary nodes that process the writes and send them to all other primary and secondary nodes to replicate
- Main problem in multi-leader is all primary nodes should be in sync with each other. which is very difficult.
- Due to this it create situation sync brain where two brain has their different data sets.
- Useful in applications in which we can continue work even if we’re offline, example is whatsapp message, google photos.
- Phone db is used to store data when we don’t have access to the internet. This is main purpose for having phone db or a secondary primary db. When ever we gets internet access it will sync with internet.

![alt text](<./assets/Screenshot (193).png>)

Conflicts can happen in Multi-LeaderReplication:
Since all the primary nodes concurrently deal with the write requests, they may modify the same data,which can create a conflict between them.

![alt text](<./assets/Screenshot (197).png>)

Conflict resolution depends on use case or user base. There can be multiple ways such as:

- First write wins
- last write wins
- custom conflict resolution algorithms.(GitHub Merge) Git conflict resolution. It ask users which one to consider git can not handle automatically.

## Leaderless Replication

Leaderless replication helps solve the following challenges faced in leader-follower replication:

- Primary node is a bottleneck for write
- Single point of failure
- Difficult to implement write scalability

In leaderless replication, all the nodes have equal weightage and can accept reads and writes requests. We can write to all database in leaderless. It works on majority wins. It has inconsistency but it offer high availability.

![alt text](<./assets/Screenshot (198).png>)

# Replication Methods

It explains how database exchange data or stay in sync with each other in any system.

There are 3 replication methods:

1. Statement-based replication
2. Write-ahead log(WAL) shipping
3. Logical (row-based) log replication

## 1. Statement-based replication

In the statement-based replication approach, the primary node saves all statements that it executes, like insert, delete, update, and so on, and sends them to the secondary nodes to perform.
It takes log files of other db and then execute exact same statement to reach consistent value. Log file contains all statement that has been executed in order. (it basically merge other dbs log files and then execute)

Used in MySQL till version 5.1

Challenges with Statement-based replication:

1. Any non-deterministic function (such as NOW()) might result in distinct writes on the follower and leader.
2. If a write statement is dependent on a prior write, and both of them reach the follower in the wrong order, the outcome on the follower node will be uncertain.

## 2. Write-ahead log

The primary node saves the query before executing it in a log file known as a write-ahead log file. It then uses these logs to copy the data onto the secondary nodes.
It first compute values of statement and then write to log file

Used in PostgreSQL and Oracle

Challenges with WAL:

1. WAL defines data at a very low level. It’s tightly coupled with the inner structure of the database engine, which makes upgrading software on the leader and followers complicated. Problem is if other dbs have different date saving formats then it can cause confusion.
   ex: Dd/mm/yyyy and other stores in mm/dd//yyyy

## 3. Logical(Row based) Replication

It selects entire row form spreadsheet, copy it and save to log files. So all secondary node will see exact value.

All secondary nodes replicate the actual data changes. For example, if a row is inserted or deleted in a table, the secondary nodes will replicate that change in that specific table.

The binary log records change to database tables on the primary node at the record level. To create a replica of the primary node, the secondary node reads this data and changes its records accordingly.

Row-based replication doesn’t have the same difficulties as WAL because it doesn’t require information about data layout inside the database engine.
