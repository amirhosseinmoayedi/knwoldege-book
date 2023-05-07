#SOA #microservice #architecture 
## What is <font color="#953734">SOA</font>?
SOA, or <font color="#953734">service-oriented architecture</font>, defines a way to <u>make software components reusable and interoperable via service interfaces</u>.

**Each service** in an SOA **embodies (includes) the code and data required to execute a complete, discrete business function** (e.g. checking a customer’s credit, calculating a monthly loan payment, or processing a mortgage application).

The **service interfaces** provide <font color="#ffff00">loose coupling</font>, meaning <u>they can be called with little or no knowledge of how the service is implemented underneath</u>,
Service interfaces are frequently defined using **Web Service Definition Language** (**WSDL**) which is a standard tag structure based on <font color="#76923c">xml</font> (extensible markup language). 

These services can be built from scratch but are often created by exposing functions from **legacy systems of record as service interfaces**.

SOA provides four different **service types**:
1. <font color="#e36c09">Functional services</font> (i.e., business services), which are critical for business applications.
2. <font color="#e36c09">Enterprise services</font>, which serve to implement functionality.
3. <font color="#e36c09">Application services</font>, which are used to develop and deploy apps.
4. <font color="#e36c09">Infrastructure services</font>, which are instrumental for backend processes like security and authentication.

Each service consists of three **components**:
1. <font color="#ffff00">The interface</font>, which defines how a service provider will execute requests from a service consumer.(like error handling, rest endpoints and etc)
2. <font color="#ffff00">The contract</font>, which defines how the service provider and service consumer should interact.(very similar to interface, provide description about service , security and policy and versioning )
3. <font color="#ffff00">The implementation</font>, which is the service code.

## SOA vs. [[Microservices]]

* SOA is an <font color="#00b0f0">integration architectural</font> style and an **enterprise-wide** concept. It enables **existing applications to be exposed over loosely-coupled interfaces**, each corresponding to a business function, that enables applications in one part of an extended enterprise to reuse functionality in other applications.

* Microservices architecture is an <font color="#31859b">application architectural</font> style and an **application-scoped** concept. It <u>enables the internals of a single application to be broken up into small pieces that can be independently changed, scaled, and administered</u>. **It does not define how applications talk to one another**—for that we are back to the enterprise scope of the service interfaces provided by SOA.

### Developer agility and productivity:
Microservices enable developers to incorporate new technologies to one part of the application **without affecting the rest of the application**. Any component can be modified, tested, and deployed independently of the others, which speeds iteration cycles.

### Scalability:
Microservices can take maximum advantage of cloud scalability—**any component can be scaled independently of the others** for the fastest possible response to workload demands and the most efficient use of computing resources.

### Resilience:
Again, thanks to decoupling, the **failure of one microservice does not impact the others**. And each microservice can perform to its own availability requirements without staking the other components or the entire application to greatest common availability requirements.

> SOA uses sync requests and ESBs and mostly the interaction between service's are implemented using SOAP, AMPQ