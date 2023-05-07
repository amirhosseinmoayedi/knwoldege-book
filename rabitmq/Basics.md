#rabbitmq #message_broker 
# <font color="#ffc000">RabbitMQ</font>
RabbitMQ is a <font color="#ffc000">message broker</font>.

> default port is `5672`

We have three common consept in RabbitMQ:
-   <font color="#92d050">Producer</font>: A program or application <font color="#92d050">that sends messages to a RabbitMQ exchange</font> for further processing and routing to one or more queues.
-   <font color="#92d050">Consumer</font>: A program or <font color="#92d050">application that subscribes to a RabbitMQ queue</font> and receives messages from the queue for further processing.    
-   <font color="#92d050">Queue</font>:<font color="#92d050"> A buffer that stores messages in RabbitMQ </font>until they are consumed by a consumer. Messages are typically produced by a producer and routed to a queue via an exchange.

Rabbitmq <u>pushes the messages to consumer</u> , consumers doesn't pull.
## <font color="#d99694">Channel</font> 
a channel is a <font color="#d99694">virtual connection inside a TCP connection</font> that allows you to <font color="#d99694">perform operations such as publishing messages or consuming messages from a queue</font>.

Channels in RabbitMQ <font color="#d99694">are lightweight and can be created and destroyed quickly</font>, allowing you to efficiently manage resources and scale your application as needed.

## <font color="#95b3d7">Que</font>
a <font color="#95b3d7">queue is a buffer that stores messages until they are consumed by a consumer</font>.
When you publish a message to RabbitMQ, you typically <font color="#95b3d7">specify a queue name or routing key</font>, which determines<font color="#95b3d7"> where the message will be routed to</font>. If a queue with that name or routing<font color="#95b3d7"> key doesn't already exist</font>, RabbitMQ <font color="#95b3d7">will automatically create</font> one for you.

### Que <font color="#00b0f0">parametes</font>:
1.  `name string`: The<font color="#00b0f0"> name of the queue</font> to declare.
2.  `durable bool`: Indicates <font color="#00b0f0">whether the queue should be persisted to disk</font> so that it<font color="#00b0f0"> survives a RabbitMQ broker restart</font>. 
3.  `autoDelete bool`: Indicates whether the <font color="#00b0f0">queue should be automatically deleted when there are no more consumers attached to it</font>. 
4.  `exclusive bool`: Indicates<font color="#00b0f0"> whether the queue should be exclusive to the connection that declares it</font>, meaning that no other connections can access the queue. 
5.  `noWait bool`: Indicates <font color="#00b0f0">whether the function should block and wait for the server to confirm the queue declaration</font>.

It's better to start the <font color="#548dd4">Que</font> both <font color="#548dd4">at publisher and subscriber</font>, because <u>we might start one before another</u>. 

## <font color="#ffff00">Exchange</font> 
an exchange is <font color="#ffff00">a message routing agent</font> that plays a crucial role in the message delivery process. I<font color="#ffff00">t receives messages from producers</font> and <font color="#ffff00">routes them to one or more queues based on the exchange type</font>, <font color="#ffff00">routing keys</font>, and <font color="#ffff00">bindings</font>

<u>Default exchange</u> identified by an **empty string** `""`.

The core idea in the<u> messaging model in RabbitMQ</u> is that the <u>producer never sends any messages directly to a queue</u>. Actually, quite often the <u>producer doesn't even know if a message will be delivered to any queue at all</u>.

### <font color="#fac08f">Binding</font>
binding is a relationship between an exchange and a queue that defines <u>how messages should be routed from the exchange to the queue</u>.

When a publisher sends a message to the exchange, it **includes a routing key** with the message. The exchange then uses **the routing key to determine which queues to route the message to**.

There are four main exchange types in RabbitMQ:
1.  **Direct**: Routes messages to queues based on a matching routing key.
2.  **Fanout**: Ignores routing keys and sends messages to all bound queues.
3.  **Topic**: Routes messages to queues based on a pattern match between the routing key and the pattern defined on the exchange.(pattern mean like regex pattern)
4.  **Headers**: Uses message header attributes instead of routing keys to bind exchanges to queues.
#### Topic
Messages sent to a topic exchange<u> can't have an arbitrary</u> **routing_key**- it must be a **list of words**, <u>delimited by dots</u>. The words can be anything, but usually they specify some features connected to the message. A few valid routing key examples: "stock.usd.nyse", "nyse.vmw", "quick.orange.rabbit". There can be as many words in the routing key as you like, up to the limit of **255** bytes.

pattern can be created by this signs:
-   * (star) can substitute for exactly one word.
-   # (hash) can substitute for zero or more words.


## Examples
### <font color="#938953">Send</font> 
messages are published via channels, this is an example in golang:
```go
ch.PublishWithContext(
	ctx,  
	"",  // exchange name
	q.Name, // routing key
	false,  // mandatory
	false,  // immediate
	amqp.Publishing{  //  The message
		ContentType: "text/plain",  
		Body: []byte(body),  
})
```
- mandatory : Indicates whether the message should be returned if it cannot be routed to a queue.
- immediate: Indicates whether the message should be delivered immediately to an available consumer.

if the message is sent to a non-existing Que RabbitMQ will **drop** the message.

A <u>message can never be sent directly to the queue</u>, it always needs to go through an <font color="#76923c">Exchange</font>.


###  <font color="#938953">receive</font>
```go
ch.Consume(
  q.Name, // queue
  "",     // consumer
  true,   // auto-ack
  false,  // exclusive
  false,  // no-local
  false,  // no-wait
  nil,    // args
)
```

- `consumer` An identifier for the consumer
- `autoAck` whether the messages should be automatically acknowledged by the consumer when they are received.
- `noLocal` Indicates whether messages published by the same connection should be ignored by the consumer. (This can be useful in scenarios where a consumer is publishing messages to the same queue that it is consuming from, and does not want to process its own messages.)
- `noWait` whether the function should block and wait for the server to confirm the consumer registration.
