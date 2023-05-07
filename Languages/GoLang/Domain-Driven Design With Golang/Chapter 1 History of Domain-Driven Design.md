#go #golang #book_summery #architecture #DDD

 ## Where Did <font color="#548dd4">DDD</font> come from ?
 engineers and architects were thinking about how to **organize their software and systems in a way that represented the problem space (<font color="#548dd4">domain</font>) they were trying to model**.
 
As software became more and more complicated, <u>it became apparent that the closer your system was to the domain, the easier it was to make changes</u>. More importantly, it was easier for other stakeholders to converse with engineers as there was less of a disconnect between the real-world model of the problem space and the system model.

## when to use it
**DDD works best when applied to large, complex systems.** A surprising number of the applications systems engineers write today are basic CRUD (short for create, read, update, and delete) applications. Applying DD development to such applications would be overkill and likely make delivery slower and more complicated.

## <font color="#953734">Pillars of DDD</font>

1. <font color="#ffc000">Ubiquitous language</font>
	Ubiquitous language is the <u>term we use to describe the process of building a common language we can use when talking about our domain</u>. This language should be spoken by everyone in the team—developers and business folk alike. **It unites the team by ensuring there is no ambiguity in communication**.

2. <font color="#76923c">Strategic design</font>
	Strategic design is a phase of the DDD process in which we **map out the business domain and define bounded contexts**. 
	The goal of strategic design is to ensure that you <u>architect your system in a way focused on business outcomes</u>. We do this by first mapping out a domain model, which is an abstract representation of the problem space.

3. <font color="#e36c09">Tactical design</font>
	Tactical design is where we begin to get into the specifics of <u>how our system will look</u>. In the tactical design phase, we begin talking about <font color="#31859b">entities, aggregates, and value objects</font>.
