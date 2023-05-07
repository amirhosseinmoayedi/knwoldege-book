#cdn #web #architecture #general 
# What is <font color="#e36c09">CDN</font> (<font color="#e36c09"> Content Delivery Network</font> )
A **Content Delivery Network (CDN)** is a globally <u>distributed network of servers that delivers web content to users based on their geographic location</u>. It caches the content at various **nodes**, so when a user requests content, it is delivered from the nearest node, improving the **speed and reliability** of content delivery. In addition to **caching**, a CDN can also provide services such as **load balancing**, **security (e.g. DDoS protection)**, and **acceleration for dynamic and interactive web applications**.

## What is a <font color="#ffff00">Point of Presence</font> (<font color="#ffff00">PoP</font>)
A Point of Presence (PoP) is a **physical location where a Content Delivery Network (CDN) provider has servers and network connections to deliver content to end users**.

## What is an <font color="#ffff00">Edge Server</font>
An Edge Server is a <u>type of server located at the edge of a Content Delivery Network (CDN)</u>, closest to the end user. Edge servers are used to cache and serve frequently requested content, **reducing the amount of data that needs to be transferred over long distances and improving the speed and reliability of content delivery**.

<u>Each PoP may have one or multiple Edge Servers,</u> which serve as a cache for the content requested by end users.


CDNs uses two types of technology:
1. DNS based routing
2. Anycast

## <font color="#31859b">DNS Based Routing</font>
DNS Based Routing is a technique used to distribute traffic across multiple servers **based on the domain name resolution process**. In this technique, the domain name resolution process is used to <u>determine the best server to handle a request based on factors such as network proximity, server load, and network performance</u>.

The DNS server returns **multiple IP addresses for a single domain name**, and the client's device selects the best IP address based on its location and network conditions. The selected IP address corresponds to the server that is best suited to handle the request.

## <font color="#31859b">Anycast</font>
Anycast is a **network routing protocol in which a single IP address is shared by multiple servers in different locations**. The IP address is announced by all servers, and the network routes traffic to the closest server based on network topology.

In Anycast, each server is connected to multiple routers and operates independently, allowing the network to route traffic to the best server in real-time. This helps to improve network performance, provide better reliability, and increase network resiliency by allowing the network to route around failures.

Both DNS based routing and Anycast are used to **improve the performance and reliability of networks** by distributing traffic across multiple servers. <u>While DNS based routing uses the domain name resolution process to determine the best server to handle a request</u>, <u>Anycast uses network topology to route traffic to the closest server</u>.

## When to use <font color="#e36c09">CDN</font>
> In general, websites and applications with a large and global audience, high traffic, and high performance requirements can benefit from using a CDN.