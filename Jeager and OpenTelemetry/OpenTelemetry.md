#observablity #microservice #telemetry #openTelemetry
# OpenTelemetry
As **distributed systems** scaled, it **became increasingly difficult for developers to see how their own services depend on or affect other services, especially after a deployment or during an outage, where speed and accuracy are critical**.

In order<u> to make a system observable, it must be instrumented</u>.
The instrumented data must then be sent to an <font color="#92cddc">Observability back-end</font>, like [[jeager]].

there were no standard for instrumenting the code so two project in <font color="#ffc000">CNCF</font> was born: 
- **<font color="#76923c">OpenTracing</font>** provided a<u> vendor-neutral API for sending telemetry data over to an Observability back-end</u>; however, it relied on developers to implement their own libraries to meet the specification.

- **<font color="#76923c">OpenCensus</font>** provided a set of<u> language-specific libraries that developers could use to instrument their code and send to any one of their supported back-ends</u>.

In the interest of having one single standard, OpenCensus and OpenTracing were merged to form <font color="#e36c09">OpenTelemetry</font>.

OpenTelemetry <u>is not an observability back-end like Jaeger or Prometheus.</u>

## <font color="#95b3d7">Instrumenting</font>
<font color="#95b3d7">Without being required to modify the source code</font> you can collect telemetry from an application using<font color="#31859b"> Automatic Instrumentation</font>.

### <font color="#c0504d">Automatic Instrumentation</font>
If applicable a language specific implementation of OpenTelemetry will provide a way to instrument your application<font color="#c0504d"> without touching your source code</font>. While the <font color="#c0504d">underlying mechanism depends on the language</font>, <font color="#c0504d">at a minimum this will add the OpenTelemetry API and SDK capabilities to your application</font>. Additionally they may add a set of Instrumentation Libraries and exporter dependencies.

### <font color="#b2a2c7">Manual Instrumentatio</font>n
Manual instrumentation in OpenTelemetry <font color="#b2a2c7">involves adding code to create and manage spans,</font> which represent the individual units of work within a distributed trace.

<font color="#b2a2c7">To manually instrument an application</font> with OpenTelemetry, the following steps are typically required:

1.  <font color="#c3d69b">Create a tracer object</font>: The tracer object is the main entry point for creating and managing spans in OpenTelemetry. The tracer object is typically created once per application and used to create spans throughout the application.

2.  <font color="#c3d69b">Create a span</font>: To create a span, you call the `StartSpan()` method on the tracer object, providing a name for the span and any relevant attributes or annotations. The `StartSpan()` method returns a span object, which can be used to add additional context and metadata to the span as needed.

3.  <font color="#c3d69b">Add context to the span</font>: Spans can include additional <font color="#c3d69b">attributes</font> and <font color="#c3d69b">annotations</font> that provide context and metadata about the operation being traced. For example, you may add information about the user or client making the request, or the database query being executed.

4. <font color="#c3d69b"> End the span</font>: When the operation being traced is complete, you call the `End()` method on the span object to mark the end of the span and record its duration.

5. <font color="#c3d69b"> Export the span</font>: <font color="#c3d69b">Once the span is complete, it is typically exported to a tracing backend for storage and analysis</font>. OpenTelemetry provides a range of exporters for popular tracing backends, including Jaeger, Zipkin, and Prometheus.

### <font color="#e36c09"> Provider and exporter </font>
a <font color="#e36c09">provider</font> is a component <font color="#e36c09">responsible for generating telemetry data</font> in your application, while an <font color="#ffc000">exporter</font> is a component <font color="#ffc000">responsible for sending that telemetry data to an external system</font> for storage or analysis.


### <font color="#92d050">Tracer</font> 
A Tracer is a component that creates and manages Spans, which represent a single unit of work in a distributed system.

### <font color="#00b050">Tracer Provider</font>
The Tracer Provider is <u>responsible for creating Tracer instances</u> based on the configuration provided, as well as <u>managing the lifecycle of those instances</u>. This includes <u>managing resources, such as memory and network connections</u>, and ensuring that the Tracers are properly configured and initialized.

The Tracer Provider is<u> typically created once per application</u>, and is responsible for creating and managing Tracers for all of the services or components within the application.

### <font color="#0070c0">Resource</font>
a resource is a set of key-value pairs that describe the environment in which an application is running.


### Type of <font color="#938953">sampling</font> 
1.  `AlwaysSample`: This sampling option <u>samples every trace</u>, regardless of the load on the system. This can be useful for debugging or for systems with low throughput.
2.  `NeverSample`: This sampling option <u>discards every trace</u>, regardless of the load on the system. This can be useful for systems with very high throughput where tracing every operation is not practical.
3.  `TraceIDRatioBased`: This sampling option samples a<u> subset of traces based on a fixed ratio</u>. For **example, a ratio of 0.5 would sample around half of the traces**. <u>This is a common sampling </u>method used in many distributed tracing systems.
4.  `ParentBased`: This sampling option samples a trace if its parent span was sampled. This can be useful for systems where spans are highly correlated and sampling one span implies that the others should also be sampled.
5.  `RateLimiting`: This sampling option uses a token bucket algorithm to limit the rate of traces that are sampled. This can be useful for systems where tracing every operation is not practical, but it's still important to sample a representative set of traces.