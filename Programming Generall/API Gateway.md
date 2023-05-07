#general #architecture #security #api-gatewat

<font color="#e36c09">API gateways</font>, sometimes called “<font color="#fac08f">edge microservices</font>", are usually used with [[microservices]].

### <font color="#ffff00">Single point</font> to Application :
An API gateway provides a single entry point for all [[API]] calls that come into an application, it sits<u> between client and backend services</u>.

Many <font color="#92cddc">monolithic</font> applications are still in use. **They primarily use API gateways to connect with external third parties, internal users or partners while providing the same security, scalability and other benefits that apply to microservices**.

## What is the difference between <font color="#d99694">API management</font> and <font color="#e36c09">API gateways</font>
While an <u>API gateway sits in front of APIs</u> — handling, routing and securing API calls — <u>API management is an overall solution that manages the entire API lifecycle and includes API gateways</u>. Another way to think about it is that API gateways are API management tools.

## <font color="#b2a2c7">SSL termination</font>
SSL termination (or SSL offloading) is <u>the process of decrypting this encrypted traffic</u>. <u>Instead of relying upon the web server</u> to do this computationally intensive work, **we can use SSL termination to reduce the load on our servers, speed up the process, and allow the web server to focus on its core responsibility of delivering web content**.

> we use ssl termination in api gateways.

## Some Imortant function of <font color="#c3d69b">API Gateway</font>:
- **authentication** and **security policy enforcements**
- **load balancing** and **[[circuit breaker]]**
- **protocol translation** and [[Services]] discovery
- **monitoring**, **Logging**, **analytics** and billing
- **caching**
- **RateLimiting**

![[Pasted image 20230226121729.png]]

## some benefits of <font color="#e36c09">API Gateway</font> explained
-   **<font color="#ffc000">Input validation</font>:** Input validation **ensures an API request has all the necessary information in the correct format before the gateway passes it along to a microservice**. If something is missing or wrong, the gateway rejects the request. When it’s validated as being correct, the gateway sends the request.

-   **<font color="#ffc000">Billing for microservices</font>:** Some businesses **monetize some of their APIs by offering a service to consumers or other companies**. The API gateway handles traffic, monitors usage for specific products or services and sends pricing information to a connected billing system. There are different types of direct monetization, including users paying as they access a service or resource, for a certain number of services or via tiers (where different services are provided at different levels). Other APIs share revenue with consumers through ad revenue share, affiliate marketing or credits to a consumer’s bill.

-   **<font color="#ffc000">Extending legacy apps</font>:** Businesses still use legacy applications that contain essential data, perform significant functions and provide value, but the apps were not written for APIs. Such older technology can have trouble handling the increasing numbers of calls from newer technologies, such as mobile, SaaS or IoT apps. They can also be hard to access. Instead of taking on a complicated cloud migration, a DevOps team can add API functionality — including benefits like rate limiting and throttling — to help modernize and extend the functionality of a legacy application.