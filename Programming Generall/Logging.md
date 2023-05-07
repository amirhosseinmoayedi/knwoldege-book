#log #programming #debuging #tracing
# <font color="#0070c0">Log</font>
Time and time again, l**ogging always proves to be an invaluable resource for investigating the root cause of a problem in contemporary computer software**. However, in the context of a <u>microservice-based architecture</u>, where requests cross service boundaries, being able to collect, correlate, and search the log entries that are emitted by each individual service is of paramount importance.

## Logging best practices
### <font color="#00b0f0">leveled logging</font>
When using leveled logging, there are two aspects you need to consider:
1. <font color="#92cddc">Selecting the appropriate log level to use for each message</font>:
2. <font color="#fac08f">Deciding which log levels to actually output</font>: For instance, perhaps you want your application to only output messages at the **INFO** and **ERROR** levels to reduce the volume of produced logs.
|Level|info|
|---|---|
|**Trace**|Only when I would be "tracing" the code and trying to find one **part** of a function specifically.|
|**Debug**|Information that is diagnostically helpful to people more than just developers (IT, sysadmins, etc.).|
|**Info** |Generally useful information to log (service start/stop, configuration assumptions, etc). Info I want to always have available but usually don't care about under normal circumstances. This is my out-of-the-box config level.(user-driven or system specific)|
|**Warn**| Anything that can potentially cause application oddities, but for which I am automatically recovering. (Such as switching from a primary to backup server, retrying an operation, missing secondary data, etc.)|
|**Error** |Any error which is fatal to the **operation**, but not the service or application (can't open a required file, missing data, etc.). These errors will force user (administrator, or direct user) intervention. These are usually reserved (in my apps) for incorrect connection strings, missing services, etc.|
|**Fatal**|Any error which is fatal to the **operation**, but not the service or application (can't open a required file, missing data, etc.). These errors will force user (administrator, or direct user) intervention. These are usually reserved (in my apps) for incorrect connection strings, missing services, etc.|

<font color="#b2a2c7">Trace</font> should not be sent to production it's a <font color="#b2a2c7">code smell</font>, never committed to your VCS.

#### Change log level
it's good practice to implement **some sort of hook to allow you to dynamically change the active log level of an application while it is executing**.

> Toggle between the configured log level and the DEBUG level when the application receives a particular signal (for example, SIGHUP). If your application reads the initial log level from a config file, you could use the same signal-based approach to force a reload of the config file.

### <font color="#e36c09">structured logging</font>
use structured logging becuse it's <font color="#e36c09">searchable</font> and <font color="#e36c09">eaiser to parse</font> can be used later for the <font color="#e36c09">data mining and analytics</font>.

Always make sure that the <font color="#ffff00">message portion of your log entries never includes variable parts</font>.
Good example:
```
level=error
message="cannot connect to server"
err="dial tcp4: lookup invalid.host: no such host" host=invalid.host
service=redirector
customer-id=42
```

#### some fields that are recomended to be part of structured logging:
* The <font color="#c3d69b">application/service name</font>.
* The <font color="#c3d69b">hostname where the application is executing</font>. When your log storage is centralized, having this field available is quite handy if you need to figure out which machine (or <font color="#c3d69b">container</font>, if you're using Kubernetes) produced the log.
* The <font color="#c3d69b">SHA of the git</font> (or your preferred VCS) branch that's used to compile the binary for the application.

<font color="#00b050">Have a Consistent Structure Across All Logs</font> because of searchability.

### <font color="#92d050">Correlation ID</font>:
 it's **a GUID (globally unique identifier) that's automatically generated for every request that the SharePoint server receives**. It's unique to each request, not each error.

it's good practice to also include a <font color="#92d050">correlation ID</font> in your log entries. In this particular scenario, the API gateway would generate a unique correlation ID for incoming requests and inject it into requests to downstream services (which then include it in their own log entries), and so on and so forth.

### Use <font color="#ffff00">Async Logging</font>
pitfall that you must be aware of is synchronous logging. The <u>golden rule here is that logging should always be considered as an auxiliary function and should never interfere with the normal operation of your service</u>.

### <font color="#d99694">Don’t Log Sensitive Information</font>
a logging security tip: don’t log sensitive information.  First, the obvious bits. Make sure you never log:
-   Passwords
-   Credit card numbers
-   Social security numbers

### <font color="#548dd4">Know What to Log</font>
* **Messages (Both Incoming and Outgoing)**
	<font color="#548dd4">Both incoming and outgoing messages</font> must be documented with [[API]] endpoint URLs, request parameters, request origin and intermediate IPs, request headers, author info, request and response bodies, business context, timestamps, and internal processing steps when components communicate via message passing.

* *Service and Function Invocations**
	<font color="#548dd4">When a service or function is called</font>, it's a good idea to report the context of the call at a lower log level, primarily for debugging. Having these logs on hand makes it much easier to explore issues with business logic, especially when we don't have the ability to connect a debugger to your program.

* **User Interactions and Business Stats**
	Every program has its own set of <font color="#548dd4">business cases and user journeys</font>, which provide a wealth of information for the system's domain specialists. Other business-related data, such as transaction volumes and active users and their stages, is useful in gaining business insights and can even be used for business intelligence.

* **Data Operations**
	In most enterprise applications, keeping a separate log statement for data-related operations with all important information like access IDs, exact service instances and role privileges used, timestamps, data layer queries, and snapshots of both previous and new states of the changed dataset is required for design logging security and compliance reasons. All data-related attempts and CRUD operations made by users, as well as by other systems and services, must be recorded in audit logs.

* **System Events**
	<font color="#548dd4">Behavior events</font>, changeover modes, inter-service communication, service instance IDs, actively serving APIs, actively listening IP and port ranges, <font color="#548dd4">configurations loaded</font>, overall service health, and everything else that aids in understanding the system's behavior must all be captured in system events.

