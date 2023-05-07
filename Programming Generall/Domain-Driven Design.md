#programming #general #architecture #DDD

## What is DDD
**Domain-driven design** (DDD) advocates <font color="#e36c09">modeling based on the reality of business as relevant to your use cases</font>.In the context of building applications, <font color="#e36c09">DDD talks about problems as domains</font>.

## <font color="#31859b">ubiquitous language</font>
DDD also emphasizes the use of "ubiquitous language" - <u>a common vocabulary that is shared between developers and domain experts</u>.

**the important part is not the patterns themselves**, but <u>organizing the code so it is aligned to the business problems</u>, and using the same business terms (<font color="#31859b">ubiquitous language</font>).

## <font color="#c3d69b">domain model</font>
One of the key concepts in DDD is the idea of a "<font color="#c3d69b">domain model</font>". This is a <u>representation of the business domain that captures its key concepts and relationships</u>, and is used to guide the development of the software. 

## <font color="#92cddc">bounded contexts</font>
Another important concept in DDD is the idea of "<font color="#92cddc">bounded contexts</font>". A bounded context is a <u>specific area of the business domain that has its own distinct language, concepts, and relationships</u>. By breaking the overall domain down into smaller, more manageable bounded contexts, developers can more easily understand and model each part of the domain.

A <font color="#ffff00">Context Map</font> in Domain-Driven Design (DDD) is a **visual representation of the relationships between bounded contexts in a system**. A bounded context is a specific, well-defined part of the business domain. The context map<u> helps to identify the interactions between bounded contexts and potential areas of conflict</u>.

There are four types of relationships:
1. partnership,
2. shared kernel
3. customer/supplier
4. conformist

By using a context map, teams can manage dependencies between bounded contexts, identify areas of overlap or conflict, and make informed decisions about how to break down a complex system into smaller, more manageable parts.
![[Pasted image 20230225160024.png]]

There are several <font color="#ffff00">design patterns</font> commonly used in Domain-Driven Design (DDD):
1. <font color="#548dd4">Repository pattern</font>: This pattern is used to <u>abstract away the details of data storage and retrieval</u>. A repository **acts as an interface** between the domain model and the underlying data storage mechanism, providing a layer of abstraction that makes it easier to swap out different data storage implementations without affecting the rest of the system.

2.  <font color="#e36c09">Factory pattern</font>: This pattern is <u>used to create complex objects with a consistent interface</u>. In DDD, factories are often used to **create domain objects that have complex construction requirement**s, or that need to be created in a particular context.

3. <font color="#31859b">Domain event pattern</font>: This pattern is <u>used to model and communicate changes in the domain model</u>. Domain events are lightweight objects that capture changes in the domain, and **can be used to trigger downstream** actions or updates in other parts of the system.

4. <font color="#92d050">Value object pattern</font>: This pattern is used to <u>represent values that have no inherent identity of their own</u>. Value objects are <u>immutable, and can be shared freely between different parts of the system without the risk of side effects</u>.

5. <font color="#938953">Aggregate pattern</font>: This pattern is used <u>to group related domain objects into a single unit of consistency</u>. Aggregates define a clear boundary around a set of related domain objects, and **provide a way to enforce consistency and transactional boundaries within the domain**.

### Transactional Boundaries
**aka atomicity in D**B
Transaction boundaries are **where a transaction begins or ends**, where within the transaction all writes to the database are ==atomic==, in that they either all complete, or are all reverted if any single write in a given transaction fails.

## Pros and Cons
| Pros | Cons |
| --- | --- |
| Focus on business value | Increased complexity |
| Modular and maintainable code | Steep learning curve |
| Improved communication and collaboration | Greater upfront design |
| Better scalability | Requires significant domain expertise |
| Testability | Can be over-engineered |

## Project layour (golang)
![[DDD_layoutjpg.jpg]]