#architecture #general #DDD 

# <font color="#ffc000">Hexagonal architecture</font>
Hexagonal architecture or <font color="#ffc000">Onion architecture</font> or <font color="#ffc000">ports and adaptors</font> architecture is <font color="#ffc000">A 4 layerd architecture</font>, which is <u>goal is abstraction of Domain (Business) logic from other parts</u>.

The Hexagonal Architecture, **is an architectural pattern that allows input by users or external systems to arrive into the Application at a Port via an Adapter, and allows output to be sent out from the Application through a Port to an Adapter**. This creates an <u>abstraction layer that protects the core of an application and isolates it from external — and somehow irrelevant — tools and technologies</u>.
![[Pasted image 20230308140000.png]]

It is Made from 4 part as one of the name suggest:
1. <font color="#e36c09">Port</font>:
	 it determines the <font color="#92cddc">interface</font> which will allow<font color="#92cddc"> foreign actors to communicate with the Application</font>, <font color="#92cddc">regardless of who or what will implement said interface</font>.
2. <font color="#e36c09">Adaptor</font>:
	<font color="#c3d69b">Move the Data from Port to Application Service or reverse</font> ,like a **middle man** 
3. <font color="#e36c09">Application Service</font>:
	<u>They do not have business logic</u> , <u>they talk to adaptors</u> and know which one to talk to when something is needded.
4. <font color="#e36c09">Domain Service</font>(Only if used With [[Domain-Driven Design]]):
	The main logics of the programme


> Adaptors never talks to other adaptors, but application services talk to multiple adaptors.


##  <font color="#00b0f0">Application Service</font>
<font color="#548dd4">The Application is the core of the system</font>, it contains the Application Services which orchestrate the functionality or the use cases. It also <font color="#548dd4">contains the Domain Model, which is the business logic embedded in Aggregates, Entities, and Value Objects</font>. The Application is represented by a hexagon which receives commands or queries from the Ports, and sends requests out to other external actors, like databases, via Ports as well.

When paired **with Domain-Driven Design**, the Application, or Hexagon, **contains both the Application and the Domain layers**, **leaving the User Interface and Infrastructure layers outside**.


## Why <font color="#d99694">Hexagon</font> ?
<font color="#d99694">Hexagon is merely to have a visual representation of the multiple Port/Adapter combinations an application might have,</font> and also to depict how the left side of the application, or ‘<font color="#d99694">driving side</font>’, has different interactions and implementations compared to the right side, or ‘<font color="#d99694">driven side</font>’,
![[Pasted image 20230308135321.png]]

## <font color="#ffff00">Implementation Details</font>
When it comes to implementation, there are a couple of important details that shouldn’t be missed:

-   <font color="#ffff00">Ports will be</font> (most of the time, depending on the language you choose) represented as <font color="#ffff00">interfaces</font> in code.
-   Driving Adapters will use a Port and an Application Service will implement the Interface defined by the Port, in this case both the Port’s interface and implementation are inside the Hexagon.
-   Driven adapters will implement the Port and an Application Service will use it, in this case the Port is inside the Hexagon, but the implementation is in the Adapter, therefore outside of the Hexagon.
![[Pasted image 20230308140625.png]]


# Why Should I Use Ports and Adapters?
There are many advantages of using the Ports and Adapters Architecture,<u> one of them is to be able to completely isolate your application logic and domain logic in a fully testable way</u>. Since it does not depend on external factors, testing it becomes natural and mocking its dependencies is easy.

**The Ports and Adapters architecture also plays along very well with Domain-Driven Design**, the main advantage <u>it brings is that it shields domain logic to leak out of your application’s core</u>. Just be vigilant of leakage between Application and Domain layers.

## Project layour (golang)
![[Hexagonal_layout.jpg]]