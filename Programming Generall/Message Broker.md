#message_broker #microservice #rabbitmq #kafka
## What is a <font color="#92cddc">Message Broker</font>?
A message broker is <u>software that enables applications, systems, and services to communicate with each other and exchange information</u>. The message broker does this by translating messages between formal messaging protocols. This allows interdependent services to “talk” with one another directly, *even if they were written in different languages or implemented on different platforms*.

>Message brokers are software modules within messaging <font color="#fac08f">middleware or message-oriented middleware (MOM)</font> solutions.

In order to provide **reliable message storage and guaranteed delivery**, message brokers often rely on a substructure or component called a <font color="#ffff00">message queue</font> that stores and orders the messages until the consuming applications can process them. In a message queue, messages are stored **in the exact order in which they were transmitted** and remain in the queue until receipt is confirmed.

> <font color="#31859b">Asynchronous messaging</font> refers to the type of inter-application communication that message brokers make possible.

## Message broker models
These are just the basic ones.
* <font color="#9bbb59">Point-to-point messaging</font>:
	* This is the distribution pattern utilized in message queues with a<u> one-to-one relationship between the message’s sender and receiver</u>. Each message in the queue is sent to only one recipient and is consumed only once. Point-to-point messaging is called for when a message must be acted upon only one time.
* <font color="#9bbb59">Publish/subscribe messaging</font>:
	* In this message distribution pattern, often referred to as **“pub/sub**,” the producer of each message publishes it to a topic, and multiple message consumers subscribe to topics from which they want to receive messages. All messages published to a topic are distributed to all the applications subscribed to it. This is a **broadcast-style** distribution method, in which there is a one-to-many relationship between the message’s publisher and its consumers.

Message brokers are a “lightweight” alternative to [[Enterprise Service Bus(ESB)]] that provide a similar functionality—a mechanism for interservice communications—more simply and at lower cost. They’re well-suited for use in the **microservices** architectures that have become more prevalent as ESBs have fallen out of favor.

## <font color="#fac08f">Kafka</font> vs [[RabbitMQ]]

### <font color="#fac08f">Kafka</font> is an open-source distributed [[Event Streaming]], facilitating raw throughput.
Kafka is a **pub/sub message bus** geared towards streams and high-ingress data replay. <u>Rather than relying on a message queue, Kafka appends messages to the log and leaves them there, where they remain until the consumer reads it or reaches its retention limit</u>.
Kafka employs a “**pull-based**” approach
Kafka is best used for streaming from A to B *without resorting to complex routing*, but with maximum throughput. It’s also ideal for<font color="#00b0f0"> event sourcing</font>, <font color="#00b0f0">stream processing</font>, and carrying out modeling changes to a system as a sequence of events. Kafka is also suitable for <font color="#00b0f0">processing data in multi-stage pipelines</font>.

### [[RabbitMQ]] is an open-source distributed message broker that facilitates efficient message delivery in **complex routing** scenarios. It’s called “distributed” because RabbitMQ typically runs as a cluster of nodes where the queues are distributed across the nodes — replicated for high availability and fault tolerance.

RabbitMQ employs a **push model** and prevents overwhelming users via the consumer configured prefetch limit. This model is an ideal approach for low-latency messaging.

Developers use RabbitMQ to process high-throughput and <font color="#00b0f0">reliable background jobs</font>, plus <font color="#00b0f0">integration</font> and <font color="#00b0f0">intercommunication</font> between and within applications. Programmers also use RabbitMQ to perform c<font color="#00b0f0">omplex routing</font> to consumers and integrate multiple applications and services with non-trivial routing logic.

### Summery of compare
>Kafka and RabbitMQ are two popular message brokers used for <font color="#92d050">asynchronous communication</font> between applications. Kafka is best for <font color="#92d050">streaming data</font>, while RabbitMQ is best for <font color="#92d050">transactional messaging</font> and routing. Kafka is <font color="#92d050">more scalable</font> and <font color="#92d050">fault-tolerant</font> than RabbitMQ, but it requires more resources to run. RabbitMQ is <font color="#92d050">easier to set up and use</font>, but it has limited scalability. Both systems have their own advantages and disadvantages, so the choice of which one to use depends on the specific requirements of the application. Kafka is better suited for <font color="#92d050">streaming data, real-time analytics, log aggregation, event sourcing, and system activity monitoring</font>; while RabbitMQ is better suited for <font color="#92d050">task queues, job scheduling, logging statistics, and system activity</font>.