#rabbitmq #message_broker #que #worker

# Wroker Que or Task Que
The main idea behind Work Queues (aka: *Task Queues*) is to <u>avoid doing a resource-intensive task</u> <font color="#ffff00">immediately</font> and having to <font color="#ffff00">wait for it to complete</font>. Instead <font color="#ffff00">we schedule the task to be done later</font>. We <u>encapsulate a </u><font color="#92d050">task</font><u> as a message and send it to the queue</u>. A <font color="#92d050">worker process running in the background</font> will pop the tasks and <font color="#92d050">eventually execute the job</font>. When you run many workers the tasks will be **shared** between them.


> goal is that each task is delivered to <font color="#ffff00">exactly one worker</font>

### <font color="#00b0f0">Round robin</font>:
Round Robin is a <font color="#00b0f0">message distribution strategy</font> used by the broker to <font color="#00b0f0">distribute messages evenly among consumers</font>. When <font color="#00b0f0">multiple consumers</font> are <font color="#00b0f0">subscribed to the same queue</font>, <u>RabbitMQ will use Round Robin to distribute messages to each consumer in turn, ensuring that each consumer receives an equal number of messages</u>.

### <font color="#92cddc">Fair dispatch</font>
is a message distribution strategy used by the broker to distribute <font color="#92cddc">messages evenly among multiple consumers, while ensuring that each consumer only receives one message at a time.</font>

unlike Round Robin, <font color="#92cddc">Fair Dispatch does not send multiple messages to a single consumer at once</font>.


they both recive gurantee the same number of messages for each consumer but fair dipatch says one at the time.
## <font color="#76923c">Message acknowledgment</font>
In order to <font color="#76923c">make sure a message is never lost</font>, RabbitMQ <font color="#76923c">acknowledgement is sent back by the consumer</font> to tell RabbitMQ that a <font color="#e36c09">particular message</font> had been <font color="#e36c09">received</font>, **<font color="#e36c09">processed</font>** and that RabbitMQ is <font color="#e36c09">free to delete</font> it.

If a **consumer dies** (its <u>channel is closed</u>, <u>connection is closed</u>) **without sending an ack**, RabbitMQ will understand that a message wasn't processed fully and will **re-queue it.** If there are other consumers online at the same time, it will then quickly redeliver it to another consumer. That way you can be sure that no message is lost, even if the workers occasionally die.A timeout (30 minutes by default) is enforced on consumer delivery acknowledgement. 

## <font color="#fac08f">Message durability</font>
When RabbitMQ <font color="#fac08f">quits or crashes it will forget the queues and messages unless you tell it not to</font>. Two things are required to make sure that messages aren't lost: we need to mark both the <font color="#fac08f">queue and messages as durable</font>.

RabbitMQ <u>doesn't allow you to redefine an existing queue with different parameters</u> and will return an error to any program that tries to do that.

### <font color="#ffff00">Not Fully Durable</font>
Marking messages as persistent<font color="#ffff00"> doesn't fully guarantee that a message won't be lost</font>. Although it tells RabbitMQ to<font color="#ffff00"> save the message to disk</font>, there is <font color="#ffff00">still a short time window when RabbitMQ has accepted a message and hasn't saved it yet</font>. Also, RabbitMQ doesn't do fsync(2) for every message -- it may be just saved to cache and not really written to the disk..

