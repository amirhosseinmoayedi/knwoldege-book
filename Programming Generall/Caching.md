#cache #architecture #performance #general 


## what is <font color="#92d050">Caching</font> ?
Caching is an **optimization technique** that you can use in your  applications to keep recent or *often-used data in memory locations* that are faster or computationally cheaper to access than their source.

## <font color="#92d050">Cache</font> strategies to evict items
| Strategy | Eviction Policy | Use Case |
|----------|-----------------|----------|
| First-In/First-Out (<font color="#ffff00">FIFO</font>) | Evicts the oldest of the entries | Newer entries are most likely to be reused |
| Last-In/First-Out (<font color="#ffff00">LIFO</font>) | Evicts the latest of the entries | Older entries are most likely to be reused |
| Least Recently Used (<font color="#ffff00">LRU</font>) | Evicts the least recently used entry | Recently used entries are most likely to be reused |
| Most Recently Used (<font color="#ffff00">MRU</font>) | Evicts the most recently used entry | Least recently used entries are most likely to be reused |
| Least Frequently Used (<font color="#ffff00">LFU</font>) | Evicts the least often accessed entry | Entries with a lot of hits are more likely to be reused |

## <font color="#548dd4">Pre-Caching</font>
The idea is to cache commonly accessed data or resources **in advance** so that when the time comes, **you can deliver it to the end-user faster**.
![[Pasted image 20230128141525.png]]
Your initial assumption about the <u>best resources for pre-caching can change.</u> Therefore, it is vital to **perform a regular analysis** of your web application’s usage pattern and derive insights about the user activity. This will help your pre-caching catalog stay relevant as your application evolves.

### <font color="#e36c09">Client-side</font>:
- <font color="#31859b">Browser-based caching</font> :  often relies on the **caching headers** to determine if a particular resource is cacheable. When a user requests a page, the browser checks its cache to see if a copy of the requested data is already available.
- <font color="#31859b">Service Worker API</font>: key static assets and materials such as HTML, CSS, JS and image files <u>that are necessary for offline access get downloaded and stored in a `Cache` instance</u>.
### <font color="#e36c09">Server-side</font>:
- **<font color="#31859b">CDNs</font>**
- **<font color="#31859b">Caching Proxy Server</font>**: It is a server that <u>sits in front of the origin server and works as a caching layer by storing a copy of the data</u>. Next time there is a request from the user, the proxy server delivers the data directly without having to make a request to the origin server. #nginx 



![[Screenshot_2023-03-26-18-24-44-741-edit_org.telegram.messenger.jpg]]