#general #architecture #microservice 

## <font color="#31859b">Event-driven Architecture</font> (EDA)
Event-driven architecture is an<u> integration model</u> **built around the publication, capture, processing, and storage (or persistence) of events**. When an application or service performs an action or undergoes a change that another application or service might want to know about, it publishes an _<font color="#c3d69b">event</font>_—<u>a record of that action or change</u>—that another application or service can consume and process to perform actions in turn.

Event-driven architecture enables a _<font color="#92cddc">loose coupling</font>_ between connected applications and services—<u>they can communicate with each other by publishing and consuming events without knowing anything about each other except the event format</u>.

## <font color="#ffc000">Transmitting Evenets</font>
There are two basic models for transmitting events in an event-driven architecture:
1. **<font color="#95b3d7">Event messaging or publish/subscribe</font>**:
		In the event messaging or <font color="#e36c09">publish/subscribe model</font>, event consumers subscribe to a class or classes of messages published by event producers.<u> When an event producer publishes an event, the message is sent directly to all subscribers who want to consume it</u>.([[Message Broker]])

2. **<font color="#b2a2c7">Event streaming</font>**:
		In the event streaming model, event producers publish _streams_ of events to a [[Message Broker]]. Event consumers subscribe to the streams, <u>but instead of receiving and consuming every event as it is published, consumers can step into each stream at any point and consume only the events they want to consume</u>.
		([[Event Streaming]])

A streaming platform offers certain characteristics a message broker does not:
- **<font color="#ffff00">Event persistence</font>:** Because consumers may consume events at any time after they are published, event streaming records are _persistent_—they are maintained for a configurable amount of time, anywhere from fractions of a second to forever. 

- **<font color="#ffff00">Complex event processing</font>:** Like event messaging, event streaming can be used for simple event processing, in which each published event triggers transmission and processing by one or more specific consumers. But, it can also be used for complex event processing, in which event consumers process entire series of events and perform actions based on the result.

## <font color="#ffc000">Benefits</font>:
1.  **<font color="#548dd4">Real-time response and analytics</font>**: Event-driven architecture enables applications to respond quickly to changing business situations by processing streams of data in real-time. This has benefits in areas such as IoT device management, security threat detection, and supply chain automation.

2.  **<font color="#548dd4">Fault tolerance, scalability, and simplified maintenance</font>**: Event-driven architecture offers loose coupling between components, making them independently deployable, testable, and updatable without interrupting service. Event persistence also enables the recovery of lost data, while scaling components becomes easy and efficient.
 
3.  **<font color="#548dd4">Asynchronous messaging</font>**: With event-driven architecture, components can communicate asynchronously, allowing for greater integration simplicity and an improved user experience. Users can complete a task in one component and move on to the next without waiting for downstream components to receive and process data.

> Not surprisingly, event-driven architecture is **widely considered best practice for [[microservices]] implementations**


There are four type of implementing the event driven architecture:


## There are four main pattern in Event Driven:
1. <font color="#76923c"> Event Notification</font>: Event Notification is a fundamental component of Event Driven Architecture. It refers to<u> the process of notifying interested parties or subscribers about events that have occurred</u>. In an event-driven architecture, an event occurs when a change happens to the system, and this event is then propagated to interested parties. **The event notification system is responsible for delivering events to the appropriate subscribers**.

2. <font color="#d99694"> Event Carried State Transfer (ECST)</font>: ECST is a<u> way to propagate state changes between services in a distributed system</u>. In ECST, instead of sending the entire state of an object or service, **only the changes that occurred to that object or service are sent**. These changes are sent in the form of events. This can help reduce the amount of data being transferred and can also help decouple services from one another.

3.  <font color="#00b0f0">Event Sourcing</font> [[Event Streaming]]: Event Sourcing is a design pattern that involves <u>storing the state of a system as a sequence of events</u>. In an event-driven architecture, events are the primary way to communicate changes in the system. Event sourcing takes this a step further by <u>using events to construct the state of the system</u>. This means that instead of storing the current state of an object, the system stores a log of events that have occurred, and the current state of the object can be reconstructed by replaying those events.

4.  <font color="#ffc000">Command Query Responsibility Segregation</font> ([[CQRS]]): CQRS is a design pattern that separates the read and write operations of a system. In a CQRS architecture, the system is divided into *two parts*: <u>a command side and a query side</u>. <u>The command side is responsible for handling write operations</u>, while the q<u>uery side is responsible for handling read operations</u>. In an event-driven architecture, CQRS can be used to separate the processing of events into separate services that are responsible for handling different types of events. For example, one service may handle events related to user authentication, while another service may handle events related to user data.

Event sourcing is used in git and ... it's a concept and widley used.