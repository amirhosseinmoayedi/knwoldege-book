#redis #database #nosql

<font color="#ff0000">Redis</font> is written in <font color="#fac08f">lua</font>.

an open-source project that basically<u> stores its data on memory</u> for most of the part, for speed. widely used as cache instead of database and it's a **no-SQL database**.

It's got many uses but mostly as:
1. <font color="#c3d69b">Chache DB</font>
2. <font color="#b2a2c7">Message Broker</font>

Redis is a **key-value** database.
It can have **nested key-value**.

The interaction with <font color="#ff0000">Redis</font> is mostly via the <u>SDK</u> or <u>CLI</u> but there are some graphical tools like <u>redisinsight</u>.

> no-sql does not mean not sql, it mean not just sql

one of the advatages that can no-sql data bases have is that the it does not mean to have a **specified schema**.

There is 4 type of no SQL databases:
1. <font color="#e36c09">Document</font>: mongo-DB ( they store JSON like data)  
2. <font color="#31859b">Graph</font>: neo4j (used for very scattered data)
3. <font color="#ffc000">Key-Value</font>: Redis
4. <font color="#d99694">Wide column</font>: Cassandra (like an SQL table but more flexible)

the <font color="#31859b">CAP</font> theory :
![https://i.stack.imgur.com/a9hMn.png](https://i.stack.imgur.com/a9hMn.png)

## Data Types
1. <font color="#c3d69b">Strings</font>
		A string represents the smallest value you can attach to a key. The <u>maximum allowed size of a string value is 512 MB</u>, containing any sequence of characters. In Redis, the <u>key part of the key-value pair is a string as well</u>.
2. <font color="#c3d69b">Lists</font>
		Redis allows you to associate an <u>ordered sequence of strings</u> to a key.
3. <font color="#c3d69b">Hashes</font>
		A Redis hash stores an <u>unordered mapping of key-value pairs</u>. A hash-key is associated with a value. The value is a Redis string that contains other key-value pairs. You cannot use other complex data structures, such as Sets, Lists, or other Hashes as values.
4. <font color="#c3d69b">Sets</font>
		A Redis set is an <u>unordered collection of unique strings</u>. As sets are not ordered, you cannot remove items from the front or end of the index as with lists. However, <u>the strings are unique, and there is no possibility that multiple instances of the same item appear within a set</u>.
5. <font color="#c3d69b">Sorted Sets</font>
		The value part of a sorted set key-value pair is composed of a unique string element (key) called a **member**, and an item (value) is called a **score**. Sorted sets map every element to a floating-point value (**score**) and use that value<u> to sort elements in a specific order</u>.
6. <font color="#c3d69b">HyperLogLogs</font>
		HyperLogLogs provide an **estimated count of unique items in a collection**. As opposed to other solutions, items in a HyperLogLogs are<u> not individually counted</u>, as that would require to keep track of previous items to avoid counting the same element twice. *Such an operation requires the amount of memory equal to the memory used to store the data*.



The <font color="#76923c">HyperLogLog</font> structure uses a much more efficient **probabilistic algorithm** that estimates a set’s size instead of counting each item. The error rate of the estimate is under 1%.

### <font color="#e36c09">Transactions</font>:
<u>a list of a query to be performed in a row</u>.
Transaction in Redis are:
 - isolated: 
	 all the commands are executed separately and another request from the client won't be executed in the middle
 - atomic: 
	all the commands will be performed or non will 



Redis configs are located in: `/etc/redis`

Redis got a **benchmark** tool for testing the server speed on a given hardware.

There are **different database in a Redis node** the default on is `0` but we can swap to other ones.


### <font color="#31859b">ACL</font> ( access control list ):
<u>create some users with a certain permission</u>: like read only.

when we create a user it's off and can't be used we should first on it and then use it , we can set which command user can use we also can add command category or remove it from user.

> we can also restrict the user access to key or even pub/sub channels.
