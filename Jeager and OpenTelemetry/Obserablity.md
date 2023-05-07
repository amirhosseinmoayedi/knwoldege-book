#observablity #log #metric #trace 
# <font color="#00b050">Obserablity</font>
Observability is a <u>concept in software engineering that refers to the ability to understand and debug complex systems</u>, such as distributed applications or microservices.
It is the <u>practice of instrumenting software to collect</u> <font color="#b2a2c7">telemetry data</font>, such as <font color="#ffc000">logs</font>, <font color="#ffc000">metrics</font>, and <font color="#ffc000">traces</font>, and using that data to gain insights into the behavior of the system.

## Pillars of observablity
1. <font color="#00b0f0">Logs</font> are<font color="#00b0f0"> textual records of events that occur within a system</font>. They can be used to track errors, warnings, and other important events, and provide a detailed history of what happened in the system. Log data can be analyzed and searched to find patterns, anomalies, and other useful information.

2. <font color="#31859b">Metrics</font> are <font color="#31859b">quantitative measurements of system behavior</font>, such as CPU usage, memory usage, and request latency. They provide a high-level view of the system's performance and can be used to identify trends and patterns over time. Metrics can be aggregated and visualized to create dashboards that give engineers a quick overview of the system's health.

3. <font color="#366092">Traces</font> are <font color="#548dd4">records of the interactions between components in a distributed system</font>. They can be used to identify performance bottlenecks, track the flow of requests through the system, and diagnose problems in complex architectures. Traces can be visualized as a graph of the system, showing how requests flow through different components and where delays or errors occur.

## Tools 
1. <font color="#76923c">Jaeger</font>: Jaeger is an open-source <font color="#76923c">distributed tracing system</font> that helps developers monitor and troubleshoot microservices-based distributed systems.
2. <font color="#00b050">OpenTelemetry</font>: OpenTelemetry is an open-source observability framework that provides a <font color="#00b050">standardized way of collecting and exporting telemetry data from different systems</font>.
3. <font color="#548dd4">Prometheus</font>: Prometheus is an open-source <font color="#548dd4">monitoring system that is designed to collect and store metrics from various sources</font>.

### <font color="#fac08f">Distributed context propagation</font>
In a<font color="#fac08f"> distributed system</font>, <font color="#fac08f">a single request can be processed by multiple services</font>, each of which <font color="#fac08f">may run on different machines</font> or even different data centers. In order to understand how a request is being processed and identify any performance or functionality issues, <font color="#fac08f">it's important to be able to trace the request as it flows through these services</font>.
this<u> allows developers to understand the behavior of the system as a whole</u>, rather than just individual services in isolation.

<font color="#ffc000">Contextual information</font>, such as **request IDs**, **timestamps**, and **user IDs**, <font color="#ffc000">can be propagated across different services</font> in a distributed system **using various techniques,** such as <u>headers in HTTP requests</u>, <u>message headers in messaging systems</u>, or <u>correlation IDs in log messages</u>.

