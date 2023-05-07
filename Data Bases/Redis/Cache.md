#redis #cdn 
Caching is typically the most effective way to boost an application's performance.
All Redis data resides in memory, which enables low latency and high throughput data access.

The `maxmemory` configuration directive is used in order to configure Redis to use a specified **amount of memory for the data set**.

```bash
CONFIG set maxmemory 100mb
```

Setting `maxmemory` to zero results into no memory limits. This is the default behavior for 64 bit systems, while 32 bit systems use an implicit memory limit of 3GB.

When the specified amount of memory is reached, it is possible to select among different behaviors, called **policies**.  

## Cache policies
|     |     |
| --- | --- |
| policy | detail |
| **noeviction** | return errors when the memory limit was reached and the client is trying to execute commands that could result in more memory to be used |
| **allkeys-lru** | evict keys by trying to remove the less recently used (LRU) keys first, in order to make space for the new data added. |
| **volatile-lru** | evict keys by trying to remove the less recently used (LRU) keys first, but only among keys that have an **expire set**, in order to make space for the new data added. |
| **allkeys-random** | evict keys randomly in order to make space for the new data added. |
| ***volatile-random*** | evict keys randomly in order to make space for the new data added, but only evict keys with an **expire set**. |
| ***volatile-ttl*** | evict keys with an **expire set**, and try to evict keys with a shorter time to live (TTL) first, in order to make space for the new data added. |
| volatile-lfu | Evict using approximated LFU among the keys with an expire set. |
| allkeys-lfu | Evict any key using approximated LFU. |

The **volatile-lru** and **volatile-random** policies are mainly useful when you want to use a single instance for both caching and to have a set of persistent keys. However it is usually a better idea to run two Redis instances to solve such a problem.

It is also worth noting that **setting an expire to a key costs memory**, so using a policy like **allkeys-lru** is more memory efficient since there is no need to set an expire for the key to be evicted under memory pressure.

## How the eviction process works:
client run a command to add data
	redis check the memory limit
		 a new command is executed if it didn't exceed 

redis does not use the exactly lru algorithm instead it uses an algorithm to simulate it in memory efficient way .<u>The reason why Redis does not use a true LRU implementation is because it costs more memory</u>. However the approximation is virtually equivalent for the application using Redis.

```bash
CONFIG set maxmemory-samples 5
```

If you think at LRU, an item that was recently accessed but is actually almost never requested, will not get expired, so the risk is to evict a key that has an higher chance to be requested in the future. LFU does not have this problem, and in general should adapt better to different access patterns.
