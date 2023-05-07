#jeager #observablity #microservice 
# <font color="#e36c09">Jager</font>
Jaeger is an open-source <font color="#e36c09">distributed tracing system</font> that helps developers <font color="#e36c09">monitor and troubleshoot microservices-based distributed systems</font>. Jaeger is designed to provide end-to-end distributed tracing, <font color="#e36c09">allowing developers to trace requests as they flow through multiple services</font>. Jaeger also provides <font color="#e36c09">visualization</font> tools, making it easy to understand and analyze the data collected by the tracing system.

### <font color="#92cddc">instrumentation</font>
in order to use Jaeger to <font color="#92cddc">collect and analyze tracing data from your application</font>, you need to <font color="#92cddc">integrate Jaeger's tracing libraries into your application's code</font>.This integration process is called <font color="#92cddc">instrumentation</font>.

Instrumentation involves adding code to your application that <font color="#95b3d7">generates tracing data and sends it to Jaeger</font>. This<font color="#95b3d7"> tracing data</font> includes information about the <font color="#95b3d7">requests flowing through your application</font>, such as the start and end times of the request, the services it passes through, and any errors that occur.

<font color="#d99694">OpenTelemetry</font> is a popular open-source framework<font color="#d99694"> for instrumenting applications to collect and export telemetry data, including tracing data</font>.

### <font color="#00b0f0">sampling</font>
sampling refers to the process of <font color="#00b0f0">selectively collecting and storing trace data, rather than capturing every single trace</font>. This is because capturing every trace can generate a huge amount of data, which can be difficult to store and process, and can also impact application performance.

In order to <font color="#00b0f0">optimize tracing data collection</font>, some distributed tracing systems, such as Jaeger, provide the ability to <font color="#00b0f0">configure sampling</font>. Sampling can be done in different ways, such as<font color="#00b0f0"> probabilistic sampling</font> (e.g., capturing only 10% of traces), or <font color="#00b0f0">based on certain criteria</font> (e.g., capturing only traces that take longer than a certain threshold).

1.  <font color="#ffff00">Probabilistic</font> sampling: Probabilistic sampling involves capturing only a certain percentage of traces, based on a random sampling algorithm. For example, a sampling rate of 0.1 means that only 10% of traces will be captured.

2.  <font color="#ffff00">Rate limiting</font> sampling: Rate limiting sampling involves capturing traces only up to a certain limit per unit of time. For example, a rate limit of 100 traces per second means that only 100 traces will be captured per second.    

3.  <font color="#ffff00">Guaranteed</font> sampling: Guaranteed sampling involves capturing every N-th trace, regardless of its characteristics.

4.  <font color="#ffff00">Adaptive</font> sampling: Adaptive sampling involves dynamically adjusting the sampling rate based on the incoming trace data. For example, if the system is experiencing high traffic, the sampling rate can be increased to capture more traces. Conversely, if the system is experiencing low traffic, the sampling rate can be decreased to capture fewer traces.
### <font color="#76923c">Collectors</font>
Collectors are components of the Jaeger tracing system that <font color="#76923c">receive trace data from instrumented applications and send it to the storage backend</font>.

### <font color="#ffff00">Span</font>
A **span** represents a<u> logical unit of work that has an operation name, the start time of the operation, and the duration</u>. Spans may be nested and ordered to model causal relationships.

A Span is a data structure that <u>contains information about the execution of an operation in the system, including its start and end times, any associated metadata</u>, and links to other Spans that are related to it.

![[Pasted image 20230415224426.png]]
### <font color="#548dd4">Trace</font>
A **trace** represents the <u>data or execution path through the system</u>.

### <font color="#c3d69b">Baggage</font>
**Baggage** is<font color="#c3d69b"> arbitrary user-defined metadata</font> (key-value pairs) that <font color="#c3d69b">can be attached to distributed context</font> and propagated by the tracing SDKs.

## Architecures
### <font color="#b2a2c7">Direct to storage</font>
In this deployment the <font color="#b2a2c7">collectors receive the data from traced applications and write it directly to storage</font>.Collectors use an in-memory queue to smooth short-term traffic peaks.
![[Pasted image 20230415225637.png]]

### <font color="#c0504d">With OpenTelemetry</font>
The Jaeger Collectors <font color="#c0504d">can receive OpenTelemetry data directly from the OpenTelemetry SDKs</font>.if you <u>already use the OpenTelemetry Collectors</u>, e.g. for gathering other types of telemetry or for pre-processing / enriching the tracing data, <u>it can be placed between the SDKs and the Jaeger Collectors</u>. The OpenTelemetry Collectors can be run as an application sidecar(in a kubernetes pod), as a host agent / daemon, or as a central cluster.

The OpenTelemetry Collector supports Jaeger’s Remote Sampling protocol and can either serve static configurations from config files directly, or proxy the requests to the Jaeger backend (e.g., when using adaptive sampling).
![[Pasted image 20230415231458.png]]

