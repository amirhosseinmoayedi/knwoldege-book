---
title: Scaling Database
updated: 2022-07-01 10:18:18Z
created: 2022-07-01 08:45:58Z
latitude: 50.11092210
longitude: 8.68212670
altitude: 0.0000
---

<img src="../../../../_resources/1d72bf97e133d5ab5a23dbdbb1a85676.png" alt="1d72bf97e133d5ab5a23dbdbb1a85676.png" width="682" height="229" class="jop-noMdConv">\

Question is not SQL or no-SQL, the question is Relational or Document key-value.

ACID or BASE

### Atomicity

[Transactions](https://en.wikipedia.org/wiki/Database_transaction "Database transaction") are often composed of multiple [statements](https://en.wikipedia.org/wiki/SQL_syntax "SQL syntax"). [Atomicity](https://en.wikipedia.org/wiki/Atomicity_%28database_systems%29 "Atomicity (database systems)") guarantees that each transaction is treated as a single "unit", which either succeeds completely or fails completely: if any of the statements constituting a transaction fails to complete, the entire transaction fails and the database is left unchanged. An atomic system must guarantee atomicity in each and every situation, including power failures, errors, and crashes.A guarantee of atomicity prevents updates to the database from occurring only partially, which can cause greater problems than rejecting the whole series outright. As a consequence, the transaction cannot be observed to be in progress by another database client. At one moment in time, it has not yet happened, and at the next, it has already occurred in whole (or nothing happened if the transaction was canceled in progress).

An example of an atomic transaction is a monetary transfer from bank account A to account B. It consists of two operations, withdrawing the money from account A and saving it to account B. Performing these operations in an atomic transaction ensures that the database remains in a [consistent state](https://en.wikipedia.org/wiki/Data_consistency "Data consistency"), that is, money is neither debited nor credited if either of those two operations fails.

### Consistency (Correctness)

[Consistency](https://en.wikipedia.org/wiki/Consistency_%28database_systems%29 "Consistency (database systems)") ensures that a transaction can only bring the database from one valid state to another, maintaining database [invariants](https://en.wikipedia.org/wiki/Invariant_%28computer_science%29 "Invariant (computer science)"): any data written to the database must be valid according to all defined rules, including [constraints](https://en.wikipedia.org/wiki/Integrity_constraints "Integrity constraints"), [cascades](https://en.wikipedia.org/wiki/Cascading_rollback "Cascading rollback"), [triggers](https://en.wikipedia.org/wiki/Database_trigger "Database trigger"), and any combination thereof. This prevents database corruption by an illegal transaction, but does not guarantee that a transaction is *correct*. [Referential integrity](https://en.wikipedia.org/wiki/Referential_integrity "Referential integrity") guarantees the [primary key](https://en.wikipedia.org/wiki/Unique_key "Unique key") – [foreign key](https://en.wikipedia.org/wiki/Foreign_key "Foreign key") relationship.

### Isolation

Transactions are often executed [concurrently](https://en.wikipedia.org/wiki/Concurrent_computing "Concurrent computing") (e.g., multiple transactions reading and writing to a table at the same time). [Isolation](https://en.wikipedia.org/wiki/Isolation_%28database_systems%29 "Isolation (database systems)") ensures that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially. Isolation is the main goal of [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control "Concurrency control"); depending on the method used, the [effects](https://en.wikipedia.org/wiki/Race_condition "Race condition") of an incomplete transaction might not even be visible to other transactions.

### Durability

[Durability](https://en.wikipedia.org/wiki/Durability_%28computer_science%29 "Durability (computer science)") guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure (e.g., power outage or [crash](https://en.wikipedia.org/wiki/Crash_%28computing%29 "Crash (computing)")). This usually means that completed transactions (or their effects) are recorded in [non-volatile memory](https://en.wikipedia.org/wiki/Non-volatile_memory "Non-volatile memory").

BASE stands for:

- **Basically Available** – Rather than enforcing immediate consistency, BASE-modelled NoSQL databases will ensure availability of data by spreading and replicating it across the nodes of the database cluster.
- **Soft State** – Due to the lack of immediate consistency, data values may change over time. The BASE model breaks off with the concept of a database which enforces its own consistency, delegating that responsibility to developers.
- **Eventually Consistent** – The fact that BASE does not enforce immediate consistency does not mean that it never achieves it. However, until it does, data reads are still possible (even though they might not reflect the reality).

==Base is much better than acid for scaling== because with scaling acid can't fucntion correctly.

in places which consistancy in the moment really matters we should use ACID.(bank , user account and etc )

<img src="../../../../_resources/2c833eb3fec9ea98ce51b63d9949cbe6.png" alt="2c833eb3fec9ea98ce51b63d9949cbe6.png" width="741" height="376" class="jop-noMdConv">

* * *

<img src="../../../../_resources/290f1084f3650f10ce87f955fbf3b7a6.png" alt="290f1084f3650f10ce87f955fbf3b7a6.png" width="684" height="347" class="jop-noMdConv">

Vertical is Assigning more Resource to Server.

Scale the Relational Databases:

<img src="../../../../_resources/2d18d477daf4c0d28cbf20645fdc09aa.png" alt="2d18d477daf4c0d28cbf20645fdc09aa.png" width="678" height="344" class="jop-noMdConv"> <img src="../../../../_resources/a2965a4b75ce5a7012387a7be56f2d2b.png" alt="a2965a4b75ce5a7012387a7be56f2d2b.png" width="685" height="376" class="jop-noMdConv">

they will have synced transactions, so updating row with one if another one is updating it will be blocked until that one is over.

<img src="../../../../_resources/86a54be626353fc59e0612a70daf93e6.png" alt="86a54be626353fc59e0612a70daf93e6.png" width="645" height="384" class="jop-noMdConv">

store hash and use application to read the data from file by hash.

<img src="../../../../_resources/333ef6a1d63679f067865984c66bfe22.png" alt="333ef6a1d63679f067865984c66bfe22.png" width="647" height="385" class="jop-noMdConv">

scaling application for each client, it's cant scale very high.

Question 3

Why is the term "NoSQL" used less and less recently?

- Many BASE-style databases are providing SQL interfaces

Which of the following techniques used to scale ACID-style databases is in a sense BASE-like?

- Read replicas

What concept in relational database design is generally ignored in BASE database systems?

- Don't allow vertical replication of string data