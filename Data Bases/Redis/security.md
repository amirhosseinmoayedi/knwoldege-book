#redis  #security 
Redis <u>is designed to be accessed by trusted clients inside trusted environments</u> This means that usually it is not a good idea to expose the Redis instance directly to the internet.

## Protected mode
when Redis is executed with the default configuration (binding all the interfaces) and without any password in order to access it, it enters a special mode called **protected mode**. In this mode **Redis only replies to queries from the loopback interfaces**, and reply to other clients connecting from other addresses with an error, explaining what is happening and how to configure Redis properly.

When the **authorization layer** is enabled, Redis will <u>refuse any query by unauthenticated clients</u>. A client can authenticate itself by sending the **AUTH** command followed by the password.

The password is set by the system administrator in clear text inside the `redis.conf` file.

In case you don't want user to access to redis config, it is possible to either rename or completely shadow commands from the command table.

```bash
rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
```

## Attacks triggered by carefully selected inputs from external clients

There is a class of attacks that an attacker can trigger from the outside even without external access to the instance. An example of such attacks are the ability to insert data into Redis that triggers pathological (worst case) algorithm complexity on data structures implemented inside Redis internals.

For instance an attacker could supply, via a web form, a set of strings that are known to hash to the same bucket into a hash table in order to turn the O(1) expected time (the average time) to the O(N) worst case, consuming more CPU than expected, and ultimately causing a Denial of Service.

New users are initialized with restrictive permissions by default