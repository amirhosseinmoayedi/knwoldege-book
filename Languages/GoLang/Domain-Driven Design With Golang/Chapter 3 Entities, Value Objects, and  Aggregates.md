#go #golang #book_summery #DDD #architecture 

## <font color="#76923c">Entities</font>
<u>entities are defined by their identity</u>.Their <u>attributes do not define them</u>, and it is expected that although their attributes may change over time, their identity will not.

Something like <font color="#ffff00">ID</font> never changes because it's the <font color="#ffff00">identifier</font>.

> It is so important to pick Right identifier and it's type so it can scale.

### A <font color="#953734">warning</font> when defining entities
Due to the focus of entities being on their identity, it is **very easy to fall into the trap of letting the database design dictate what your domain model will look like**. This can lead to what is known as an <font color="#b2a2c7">anemic domain model</font>.

## <font color="#b2a2c7">Anemic models</font>
Anemic models <u>have little or no domain behavior as part of their design</u>. This means that you are not getting the full benefit of DDD.

It is quite **easy to diagnose anemic models and course-correct them if they’re identified early enough**:
	If your model <u>has mostly public getter and setter </u>functions,<u> no business logic, or depends on various clients to implement the business logic</u>, you probably have an anemic model

> Even in  simple examples, we can see the benefit of our entity having some business logic. these can be **more cleare name** and **some validation** for public setter and getter and some err raised if the entities was used wrongly and etc. 

**Don't apply the database design same as the domain design and vice versa**.
### <font color="#d99694">note on ORMs</font>
If you want to use an ORM,<font color="#d99694"> ensure it does not control how you write your entities in your DDD context</font>; otherwise, you may end up with an anemic model.
We also <font color="#d99694">want to keep the coupling between our entity and ORM to a minimum</font>. Therefore, I recommend you use an<font color="#d99694"> adaptor layer to decouple your ORM and DDD entity layer</font>.

## <font color="#fac08f">value objects</font>
Value objects are, in some ways, <font color="#fac08f">the opposite of entities</font>. With value objects, we want to assert that two objects are the same given their values. <font color="#fac08f">Value objects do not have identities and are often used in conjunction with entities and aggregates to enable us to build a rich model of our domain</font>.

```go
type Point struct { x int y int }

func NewPoint(x, y int) *Point {
	return &Point{ x: x, y: y }
	 }

a := chapter3.NewPoint(1, 1)
b := chapter3.NewPoint(1, 1)
// a is not equall to b becasue the identifier which is the memory location is different 

func NewPoint(x, y int) Point { 
	return Point{ x: x, y: y }
	 }
a := chapter3.NewPoint(1, 1)
b := chapter3.NewPoint(1, 1)
// a is equall to b because the values are equall
// They are value objects; we can treat them equally if their values are equal
```
Notice how, in the point class, x and y are lowercase? **This is to stop them from being exported and mutated. It is recommended that value objects remain immutable to prevent any unexpected behavior**.

Value objects should be <font color="#b7dde8">replaceable</font>.

### <font color="#95b3d7">side-effect-free</font>
A method of an object can be designed as a Side-Effect-Free Function . A function is **an operation of an object that produces output but without modifying its own state**. Since no modification occurs when executing a specific operation, that operation is said to be side-effect free.

By following the <u>principles of immutability and side-effect-free functions, we have made our value objects easier to reason about and to write unit tests for</u>. We can write very simple tests with multiple different inputs with predictable outputs. This will help us with the long-term maintenance of the system.

## when to use **entity and value object**
<u>We should aim to use value objects as much as possible when modeling our domain</u>. This is because **they are the safest constructs we can use when implemented correctly**. We do not have to worry about consumers incorrectly modifying our instance in a way we did not intend.

### If the answers to all these questions are yes, a value object is probably right for your use case
* Is it possible for me to treat this object as immutable?
* Does it measure, quantify, or describe a domain concept?
* Can it be compared to other objects of the same type by its values?

> Try and make everything a value object to start with until it does not fit your use case. At that point, it can be upgraded to an entity

## The <font color="#e36c09">aggregate</font> pattern
Aggregates are probably one of the hardest patterns of domain-driven design and are, therefore,<font color="#e36c09"> often implemented incorrectly</font>. This isn’t necessarily bad if it helps you organize your code, but<font color="#e36c09"> in the worst case, it may hinder your development speed and cause inconsistencies</font>.

the aggregate pattern <font color="#e36c09">refers to a group of domain objects that can be treated as one for some behaviors</font>.

Example:
![[Pasted image 20230227155821.png]]

> Often, aggregates are confused with **data structures used for collections of data**, such as arrays, maps, and slices. These are not the same thing. While an aggregate may use these collections, an aggregate is a DDD concept and, therefore, will usually contain multiple collections, fields, functions, and methods.

the job of an aggregate pattern is to <u>act as a transaction boundary for the domain objects within</u>. Loading, saving, editing, and deleting should happen to all objects within the aggregate or not at all.

Before trying to cluster our domain models into aggregates, we need to find our <font color="#31859b">bounded context’s invariants</font>. An <font color="#31859b">invariant</font> is <font color="#31859b">simply a rule in our domain that must always be true</font>. 
For example, we may say that in our system, for an order to be created, we must have the item in stock. This is a business invariant. If we do not have an item in stock, we cannot promise it to customers.

**we want any changes to our aggregate to be immediate and atomic**.

Generally, we should aim for **small aggregates**. **Keeping aggregates small will help make our system more scalable, improve performance, and give transactions more chance of success**.