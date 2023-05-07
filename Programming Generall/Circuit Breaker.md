#general #architecture #api-gateway #microservice #circuit_breaker
## <font color="#938953">Circuit breakers Simple</font>
Circuit breakers are a technique developed by software engineers<u> to enable developers to quickly and easily disable certain functions within a software project in order to troubleshoot bugs or other problems</u>. It helps the developer<u> avoid needing to shut down the entire program by only removing the functionality that is not working correctly</u>.

## <font color="#c3d69b">Circuit Breaker pattern</font>
### <font color="#d99694">the problem</font>
In a **distributed environment**, calls to <u>remote resources and services</u> can fail due to transient faults, such as slow network connections, timeouts, or the resources being overcommitted or temporarily unavailable. These faults typically correct themselves after a short period of time, and a robust cloud application should be prepared to handle them by using a strategy such as the <font color="#fac08f">Retry pattern</font>.

**there can also be situations where faults are due to unanticipated events, and that might take much longer to fix.**

if a service is very busy, failure in one part of the system might lead to <font color="#ffff00">cascading failures</font>.
### <font color="#00b050">the solution</font>
The <font color="#c3d69b">Circuit Breaker pattern</font>, can <font color="#c3d69b">prevent an application from repeatedly trying to execute an operation that's likely to fail</font>. Allowing it to continue without waiting for the fault to be fixed or wasting CPU cycles while it determines that the fault is long lasting.

The Circuit Breaker pattern <font color="#00b050">also enables an application to detect whether the fault has been resolved</font>. If the problem appears to have been fixed, the application can try to invoke the operation.

A circuit breaker <u>acts as a proxy</u> for operations that might fail.
The proxy can be implemented as a state machine with the following states that <font color="#ffff00">mimic the functionality of an electrical circuit breaker</font>:
-   **<font color="#e36c09">Closed</font>**: <u>The request from the application is routed to the operation</u>. The proxy maintains a count of the number of recent failures, and if the call to the operation is unsuccessful the proxy **increments this count**. If the number of recent failures exceeds a specified threshold within a given time period, the proxy is placed into the **Open** state. At this point the proxy starts a timeout timer, and when this timer expires the proxy is placed into the **Half-Open** state.

    > The purpose of the timeout timer is to give the system time to fix the problem that caused the failure before allowing the application to try to perform the operation again.


-   **<font color="#e36c09">Open</font>**: <u>The request from the application fails immediately and an exception is returned to the application</u>.

-   **<font color="#e36c09">Half-Open</font>**: <u>A limited number of requests from the application are allowed to pass through and invoke the operation</u>. If these requests are successful, it's assumed that the fault that was previously causing the failure has been fixed and the circuit breaker switches to the **Closed** state (the failure counter is reset). If any request fails, the circuit breaker assumes that the fault is still present so it reverts back to the **Open** state and restarts the timeout timer to give the system a further period of time to recover from the failure.

    > The **Half-Open** state is useful to prevent a recovering service from suddenly being flooded with requests. As a service recovers, it might be able to support a limited volume of requests until the recovery is complete, but while recovery is in progress a flood of work can cause the service to time out or fail again.


![[Pasted image 20230227142455.png]]

## Use this pattern:
-   **To prevent an application from trying to invoke a remote service or access a shared resource if this operation is highly likely to fail**.