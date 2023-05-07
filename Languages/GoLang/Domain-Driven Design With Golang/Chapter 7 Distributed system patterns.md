#go #golang #book_summery #CQRS #cap

## <font color="#fac08f">CAP</font>
You often must make <font color="#fac08f">trade-offs in the preceding categories to build a distributed system</font>. A famous theorem taught in computer science classes is called the<font color="#fac08f"> CAP theorem</font>. The CAP theorem states <font color="#fac08f">you must pick two of the following categories to make guarantees about, and the third you must accept will suffer</font>.
These are:
* <font color="#92cddc">Consistency</font>: Every read receives the most up-to-date information or an error. 

* <font color="#92cddc">Availability</font>: Every request receives a non-error response, but it may not receive the most up-to-date information. 

* <font color="#92cddc">Partition tolerance</font>: <u>The system continues to operate even if there are network issues happening</u> (such as packets being dropped). **In the event of a failure** here, the system designer must make a choice between: 
	1. <u>Canceling the operation</u> and thus <u>decreasing the availability but ensuring consistency </u>
	2. <u>Proceeding with the operation</u> and thus providing <u>availability but risking inconsistency</u>

Examples are :
> Mongo is considerd CP while Casandra us AP.

<font color="#ffc000">In Real Life, it's not like this, and systems are designed somehow so that they have percent of availability and consitancy in the same time</font>.

One real-world example of a system that balances consistency and availability is Amazon's DynamoDB. DynamoDB is a NoSQL database service that provides fast and predictable performance with seamless scalability. It allows developers to choose between "eventual consistency" and "strong consistency" for read operations. With eventual consistency, reads might not always reflect the most recent write, but it provides low latency and high availability. With strong consistency, reads always reflect the most recent write, but it might result in higher latency or lower availability.

## [[CQRS]]:
<u>CQRS is not necessary for DDD</u>, but the sort of <u>complex systems that benefit from DDD may also benefit from exploring CQRS</u>:
> both are there to help you model and **manage complexity**.

Bertrand Meyer, the creator of the CQRS pattern, suggests that we follow a few simple rules while implementing CQRS. 
 * Every method should be a command that **performs an action or a query that answers a question**. However, <u>no method should do both</u>. 
 * Asking a question should not change the answer; <u>queries should not mutate</u>.

We can adapt these to Golang, as follows:
* <font color="#92cddc">If a method modifies the state of the receiver struct or database, it is a command and should return an error or nil </font>
* <font color="#c3d69b">If a method returns a value, it should not modify the database or its receiver struct</font>

It might be tempting to try to enforce this through an interface, as follows:
 ```go
type Commander interface { 
	Command(ctx context.Context, args ...interface{}) error
}
type Querier interface {
	Question(ctx context.Context, args ...interface{})(interface{}, error)
}
```
> But I really do not recommend this.

In monolithic systems, I rarely believe the CQRS pattern is the best option for managing complexity unless implemented perfectly. However, it can work fantastically well for event-based systems

## <font color="#76923c">Dealing with failure</font> 
<font color="#92d050">we must expect that our distributed system will fail due to both factors outside of our contro</font>l and edge-case failure modes that we accept can happen from time to time, but we accept that risk in favor of delivery speed.
**there are some method for to deal with failure**.
### <font color="#548dd4">Two-phase commit</font> (2PC)
it is near impossible to create distributed transactions and commit atomically. One approach to solve this is to split our work into two phases: 
* <font color="#548dd4">Preparation phase</font>: We<u> ask each of our sub-systems if it can promise to do the workload we want to complete</u>. 
* <font color="#548dd4">Completion phase</font>:<u> Tell each sub-system to do the work it just promised to do</u>.

In the <font color="#8db3e2">preparation phase</font>, each of the sub-systems will complete whatever action is necessary to ensure it can keep its promise. <font color="#8db3e2">In a lot of situations, this is putting a lock around a resource</font>. If any of the participants cannot make this commitment, or <font color="#8db3e2">if a specified time interval passes without hearing from the coordinator, the workload is aborted</font>.
![[Pasted image 20230307212037.png]]
> 2PC is a useful pattern to be aware of when building a domain-driven system.

![[Pasted image 20230417213612.png]]

Here the coordinator is to make sure the calls to seriveA and serviceB <font color="#92cddc">happen none or both</font>.

-   provide prepare() interface on both seriveA and serviceB
-   call both prepare() function and get the result
-   make a commit or rollback based on the response


The <font color="#ffff00">biggest disadvantage of the 2PC is the fact that it’s a blocking protocol</font>. This means in the best case<font color="#ffff00"> we lose some of the concurrency ability within our system</font>, and in the worst case, no work can be completed at all until the lock is released (either manually or when a pre-specified threshold expires).

 the use cases are very limited to:
-   <font color="#92cddc">cross multiple databases commit</font>
-   simple and fast services. and most of the cases are expected to succeed (retry is cost, taking up a connection, and blocking request again)
-  <font color="#92cddc"> Could be used in Synchronizes process and does not care about performance.</font>

### <font color="#00b050"> TCC — Try-Confirm-Cancel</font>
![[Pasted image 20230417213818.png]]

The first thing is Every service needs to implement this contract: 3 <font color="#00b050">interfaces try, confirm, and cancel</font> for the caller.

-   <font color="#ffc000">application</font>. which is the <font color="#ffc000">business app</font>. calls try() service to <font color="#ffc000">attempt to make a transaction</font> (precheck, prepare resource, etc)
-   <font color="#ffc000">Transaction manager</font>. <u>Track transactions happening and store logs</u>. then make a rollback or commit based on the service call. It talks to different services and provides a wrapper for the business app to confirm or cancel.

### The <font color="#e36c09">saga pattern</font>
The saga pattern aims to <font color="#e36c09">allow us to achieve consistency within a distributed system without preventing concurrency</font>.
The basic principle of the saga pattern is a simple one:
<font color="#e36c09">for each action we take within our system, we also define a compensating action that we call in the event we need to roll back</font>.
![[Pasted image 20230307212123.png]]
<u>we can combine the saga pattern with an EDA</u> (that we mentioned previously) and emit an event for compensating control. This means it can be retried by consumer services at their own pace and using their own patterns.