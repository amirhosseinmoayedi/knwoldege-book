#general #event_driven #architecture #CQRS

## Command and Query Responsibility Segregation (<font color="#ffc000">CQRS</font>)
A <u>pattern that separates read and update operations for a data store</u>.

### Problem
In traditional architectures, the **same data model is used to query and update a database**. That's simple and **works well for basic CRUD operations**. In more complex applications, however, **this approach can become unwieldy**.

![[Pasted image 20230221143642.png]]

>  Lock _contention_ or <font color="#76923c">Data contention</font>  happens when multiple processes are trying to access the same _data_ at the same time.

#### Cons
-   There is often a <u>mismatch between the read and write representations of the data</u>, such as additional columns or properties that <u>must be updated correctly even though they aren't required as part of an operation</u>.

-   <font color="#76923c">Data contention</font> can occur when operations are performed in parallel on the same set of data.

-   The traditional approach can have a <u>negative effect on performance due to load on the data store and data access layer</u>, and the complexity of queries required to retrieve information.

-   Managing <u>security and permissions can become complex</u>, because each entity is subject to both read and write operations, which might expose data in the wrong context.

## Soloution That CQRS Provide

> A <font color="#e36c09">Data Transfer Object</font> is an object that is used to encapsulate data, and send it from one subsystem of an application to another.

-   Commands<u> should be task-based</u>, rather than data centric. ("Book hotel room", not "set ReservationStatus to Reserved").
-   Commands may<u> be placed on a queue for asynchronous</u> processing, rather than being processed synchronously.
-   <u>Queries never modify the database. A query returns a</u> <font color="#e36c09">DTO</font><u> that does not encapsulate any domain knowledge</u>.

![[Pasted image 20230221144105.png]]

 However, **one disadvantage is that CQRS** code **can't automatically be generated from a database schema using scaffolding mechanisms such as O/RM tools**.

 ><font color="#92cddc">materialized view</font>: Generate prepopulated views over the data in one or more data stores when the data isn't ideally formatted for required query operations.
  
For <u>greater isolation, you can physically separate the read data from the write data.</u> In that case, the **read database can use its own data schema that is optimized for queries**, it can store a <font color="#92cddc">materialized view</font> of the data, in order to avoid complex joins or complex O/RM mappings, **It might even use a different type of data store**.
For example, the write database might be relational, while the read database is a document database.

![[Pasted image 20230221144529.png]]

## Benefits of CQRS include:
-   **<font color="#ffc000">Independent scaling</font>**. CQRS allows the read and write workloads to scale independently, and may result in fewer lock contentions.

-   **<font color="#ffc000">Optimized data schemas</font>**. The read side can use a schema that is optimized for queries, while the write side uses a schema that is optimized for updates.

-   **<font color="#ffc000">Security</font>**. It's easier to ensure that only the right domain entities are performing writes on the data.

-   **<font color="#ffc000">Separation of concerns</font>**. Segregating the read and write sides can result in models that are more maintainable and flexible. Most of the complex business logic goes into the write model. The read model can be relatively simple.

-   **<font color="#ffc000">Simpler queries</font>**. By storing a <font color="#92cddc">materialized view</font> in the read database, the application can avoid complex joins when querying.

## Some challenges of implementing this pattern include:
-   **<font color="#0070c0">Complexity</font>**. The<u> basic idea of CQRS is simple</u>. But<u> it can lead to a more complex application design</u>, especially if they include the Event Sourcing pattern.

-   **<font color="#0070c0">Messaging</font>**.<u> Although CQRS does not require messaging, it's common to use messaging to process commands and publish update events</u>. In that case, the application must handle message failures or duplicate messages.

-   **<font color="#0070c0">Eventual consistency</font>**. If you separate the read and write databases, the read data may be stale. The read model store must be updated to reflect changes to the write model store, and it can be difficult to detect when a user has issued a request based on stale read data.
		
The CQRS pattern is often used along with the Event Sourcing pattern.When used with the Event Sourcing , the store of events is the write model, and is the official source of information.

## Command vs Query
**a <font color="#ffc000">Query</font> should not modify anything, just return the data. A <font color="#ffc000">command</font> is the opposite one: it should make changes in the system, but not return any data.**


## Project layour (golang)
![[CQRS_layoutjpg.jpg]]