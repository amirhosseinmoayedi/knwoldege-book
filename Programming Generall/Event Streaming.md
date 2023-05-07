#microservice #architecture #event_driven
#esp #event_streaming 

## What is <font color="#e36c09">Event</font> ?
In the context of applications, **events simply are things that happen within a software system**, <u>Each event  can trigger one or more actions or processes in response</u>. 

## What is <font color="#ffff00">event streaming</font>: Event-driven architecture
One of the inherent challenges with **[[microservices]]** is the <font color="#953734">coupling</font> that can occur between the services.
> An event-driven architecture utilizes a tell, don’t ask approach( Push Base not pull base ).

Using event streams in this way can deliver a number of <font color="#00b050">benefits</font>, including:
* Systems that more closely <font color="#366092">mimic real-world processes</font>, because software teams have carefully considered the optimal flow.
* Increased utilization of <font color="#366092">scale-to-zero functions</font> (or **serverless computing**), <u>because more services can remain idle until they’re needed</u>.
* Improved <font color="#00b0f0">flexibility</font>, **because new requirements or features can be captured in loosely coupled microservices instead of rewriting the business logic of a large monolith**.

Modern stream processing systems such as **Apache Kafka** can also act as a stateful <font color="#ffff00">source of truth</font> about the business. Because they *can store and process events—while also holding onto historical data—systems can analyze and aggregate data in real time without reaching out to external data sources.*

