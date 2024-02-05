# Redis

Redis (Remote Dictionary Server) is an open-source, in memory data structure store, used as a database, cache and message broker. It supports various data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperlogs, geospatial indexes and streams. One of the key features of Redis is its ability to store key-value pairs in memory, which allows for very fast read and write operations. It also offers replication,  transactions and different levels of on-disk persistence.

## - How run it using Docker

> docker run --name redis-server -d redis redis-server --save 60 1 --loglevel warning
> docker run -it --link redis-server:redis --rm redis redis-cli -h redis -p 6379 

## - It is really used ?

Redis is widely used in the industry. Many big companies as Twitter, Github, Snapchat and Stackoverflow use Redis for caching, session storadge , messaging and other use cases. It's a popular choice due to its performance, flexibility, and wide variety of use cases.

## - Examples of use:

### LRU eviction

To enable LRU (Least Recently Used) eviction, set the **maxmemory-policy** in your redis. The LRU eviction policy allows Redis to automatically remove less recently used keys when a memory limit has been reached, allowing more space for the addition of new data. This is especially useful in caching scenarios where you might want to discard older data to make room for the more recent or frequently accessed data. 

To configure Redis to use LRU eviction, you need to:

1.  Set a max memory limit. Redis won't start evicting keys until this limit is reached. You can do this in your redis.conf file or via the `CONFIG SET` command. For example, to set a max memory limit of 100mb:

> CONFIG SET maxmemory 100mb

2.  Set the `maxmemory-policy` to `allkeys-lru` (or `volatile-lru` if you only want to evict keys with an `expire` set). Again, you can do this in your redis.conf file or via the `CONFIG SET` command:

> `CONFIG SET maxmemory-policy allkeys-lru`

Now, whenever you reach the memory limit, Redis will start evicting the least recently used keys.
It's important to note that Redis's implementation of LRU eviction is not a perfect LRU, but rather an approximation. This is due to the fact that keeping track of the last access time for each key can be expensive, so Redis samples a few keys and evicts the least recently used one out of those keys.
For example, let's say you've set a memory limit of 100mb and you've set `maxmemory-policy` to `allkeys-lru`. You start adding data to Redis until you reach the memory limit. At this point, when you try to add a new key, Redis will sample a few keys from the ones it currently has, find the one that was least recently used, and evict that key to make room for the new one.

### Transactions

Transactions in Redis are a way to execute a group of commands in a single step. Redis transactions provide atomicity, which means that either all the commands succeed or none do. You can group commands into a transaction with the `MULTI`  command. This command tells Redis that you are starting a transaction. After this command, every subsequent command is not excuted immediately but is queued up. When you're ready to run your transaction, you use the `EXEC` command, nd Redis will run all your queued commands on the go.

Ex:

```
MULTI
SET book1 "Real"
SET book2 "Madrid"
EXEC
```
If a failure occurs during the execution of the transaction, Redis will stop executing the transaction and none of the remaining commands will be executed.

`WATCH`: This command allows you to watch a key and if the key is modified by another client before the transaction executes, the transaction will be aborted. This is used to handle concurrent operations and is a form of optimistic locking.

```
WATCH book1
MULTI
SET book1 "The Hobbit"
EXEC
```

### Caching

Caching is a technique used to store copies of data in high-speed access areas to improve data retrieval times. This technique is most commonly applied in scenarios where data retrieval from the main data source (like a database or an API) is slow or expensive in terms of resources. Redis, with its high-speed in-memory store, is a perfect fit for such caching applications. It allows you to store key-value pairs in memory, which can be accessed very quickly compared to traditional databases.

Ex: 

> `SET user:1001 "{ 'name':'John', 'age': 30, 'city':'New York' }"`

In this example, we're caching user information. The key `user:1001` represents a user with ID 1001, and the value is a stringified JSON object with the user's details. When you need to retrieve this user's details, you can get it from Redis instead of your primary database. 

> `GET user:1001`

The response will be instant since Redis stores data in memory. To automatically remove old data from the cache, you can use the `EXPIRE` command, which sets a key to be automatically deleted after a specified number of seconds:

> `EXPIRE user:1001 3600`  
 
The key will be deleted after 3600 seconds (1 hour).

Remember that caching isn't suitable for all types of data. It's best used for data that:

-   Is frequently accessed
-   Doesn't change very often
-   Is expensive or slow to fetch from the main data source
-   Can be regenerated if lost or expired


## - Store techniques


### Strings

##### 1. GET

Retrieves the value of a key. If the key does not exist, then it returns `nil`.

```bash
> SET mykey "Hello, Redis"
> GET mykey
"Hello, Redis"
```

##### 2. GETRANGE

Gets the substring of the stored value at the specified key. The substring is defined by the indices provided.

```bash
> SET mykey "Hello, Redis"
> GETRANGE mykey 0 4
"Hello"
```

##### 3. MSET

Sets multiple keys to multiple values.

```bash
> MSET key1 "Hello" key2 "Redis"
> GET key1
"Hello"
> GET key2
"Redis"
```

##### 4. STRLEN

Returns the length of the string value stored in the key.
```bash
> SET mykey "Hello, Redis"
> STRLEN mykey
11
```

##### 5. INCR / INCRBY / DECRBY

`INCR` increments the integer value of a key by one. `INCRBY` increments the integer value of a key by the given number. `DECRBY` decrements the integer value of a key by the given number.

```bash
> SET mycounter 100
> INCR mycounter
101
> INCRBY mycounter 50
151
> DECRBY mycounter 20
131
```

##### 6. SETEX

Sets the value of a key and associates the key with a specified timeout.

```bash
> SETEX mykey 10 "Hello, Redis"
> GET mykey
"Hello, Redis"
```

In this example, the value "Hello, Redis" will automatically be deleted from the key `mykey` after 10 seconds.


### Lists

##### 1. LPUSH

Inserts all the specified values at the head of the list stored at key.

bashCopy code

`> LPUSH mylist "World"
> LPUSH mylist "Hello"
> LRANGE mylist 0 -1
["Hello", "World"]` 

##### 2. LRANGE

Returns the specified elements of the list stored at key.

bashCopy code

`> LPUSH mylist "World"
> LPUSH mylist "Hello"
> LRANGE mylist 0 -1
["Hello", "World"]` 

##### 3. LLEN

Returns the length of the list stored at key.

bashCopy code

`> LPUSH mylist "World"
> LPUSH mylist "Hello"
> LLEN mylist
2` 

##### 4. RPOP

Removes and returns the last element of the list stored at key.

bashCopy code

`> LPUSH mylist "World"
> LPUSH mylist "Hello"
> RPOP mylist
"World"` 

##### 5. LPOP

Removes and returns the first element of the list stored at key.

bashCopy code

`> LPUSH mylist "World"
> LPUSH mylist "Hello"
> LPOP mylist
"Hello"` 

##### 6. LINSERT 

Inserts value in the list stored at key either before or after the reference value pivot.

bashCopy code

`> RPUSH mylist "Hello"
> RPUSH mylist "World"
> LINSERT mylist BEFORE "World" "Redis"
> LRANGE mylist 0 -1
["Hello", "Redis", "World"]` 

##### 7. RPUSH

Inserts all the specified values at the tail of the list stored at key.

bashCopy code

`> RPUSH mylist "Hello"
> RPUSH mylist "World"
> LRANGE mylist 0 -1
["Hello", "World"]` 

##### 8. LREM

Removes the first count occurrences of elements equal to element from the list stored at key.

bashCopy code

`> RPUSH mylist "Hello"
> RPUSH mylist "Hello"
> RPUSH mylist "World"
> LREM mylist 1 "Hello"
> LRANGE mylist 0 -1
["Hello", "World"]`

##### 8. Sort 

The `SORT` command in Redis can be used to sort the elements of a list, a set, or a sorted set. Here is an example:

bashCopy code

`> RPUSH mylist 5 3 9 1
> SORT mylist
["1", "3", "5", "9"]` 

In this example, the `RPUSH` command is used to insert the numbers 5, 3, 9, and 1 into the list `mylist`. Then, the `SORT` command is used to sort the elements in the list.

The `SORT` command sorts the elements in ascending order by default. If you want to sort the elements in descending order, you can use the `DESC` modifier:

bashCopy code

`> RPUSH mylist 5 3 9 1
> SORT mylist DESC
["9", "5", "3", "1"]` 

By default, the `SORT` command doesn't store the result, it just returns it. If you want to store the result in a new key, you can use the `STORE` modifier:

bashCopy code

`> RPUSH mylist 5 3 9 1
> SORT mylist STORE sortedlist
> LRANGE sortedlist 0 -1
["1", "3", "5", "9"]` 

In this example, the sorted list is stored in the new key `sortedlist`. The `LRANGE` command is then used to display all the elements in `sortedlist`

### Set

In Redis, a set is a collection of unique, unsorted string elements. The uniqueness of elements within a set means that each element can only occur once, no matter how many times it is added.

Here are some important characteristics of sets in Redis:

-   **Uniqueness:** All elements in a set are unique. If you try to add an element that already exists, Redis will simply not add it again and will not return an error.
    
-   **Unordered:** The elements in a Redis set are not ordered. This means that when you retrieve the elements, they won't necessarily be in the order in which you added them.
    
-   **High Performance:** Redis sets offer constant time complexity for adding, removing, and testing elements. You can perform these operations quickly, even on large sets.
    
-   **Operational Commands:** Redis provides a wide array of commands to manipulate sets and perform operations like union, intersection, and difference, making it powerful for certain use cases.


Real use can be found in a variety of real-world applications. Let's consider a social networking application as an example. In a social networking application, you might want to store and retrieve the set of friends for a particular user. Because friends must be unique (a user can't be friends with the same person twice), and order typically doesn't matter, a set is a good choice for this data.

##### 1. SADD

Adds one or more members to a set.

bashCopy code

`> SADD myset "Hello"
> SADD myset "World"
> SMEMBERS myset
["Hello", "World"]` 

##### 2. SMEMBERS

Returns all the members of the set value stored at key.

bashCopy code

`> SADD myset "Hello"
> SADD myset "World"
> SMEMBERS myset
["Hello", "World"]` 

##### 3. SREM

Removes one or more members from a set.

bashCopy code

`> SADD myset "Hello"
> SADD myset "World"
> SREM myset "World"
> SMEMBERS myset
["Hello"]` 

##### 4. SISMEMBER

Determines if a given value is an element of the set.

bashCopy code

`> SADD myset "Hello"
> SISMEMBER myset "Hello"
(integer) 1
> SISMEMBER myset "World"
(integer) 0` 

##### 5. SCARD

Gets the number of members in a set.

bashCopy code

`> SADD myset "Hello"
> SADD myset "World"
> SCARD myset
(integer) 2` 

##### 6. SINTER

Returns the members of the set resulting from the intersection of all the given sets.

bashCopy code

`> SADD myset1 "Hello"
> SADD myset1 "World"
> SADD myset2 "World"
> SINTER myset1 myset2
["World"]` 

##### 7. SUNION

Returns the members of the set resulting from the union of all the given sets.

bashCopy code

`> SADD myset1 "Hello"
> SADD myset1 "World"
> SADD myset2 "Redis"
> SUNION myset1 myset2
["Hello", "World", "Redis"]` 

##### 8. SDIFF

Returns the members of the set resulting from the difference between the first set and all the successive sets.

bashCopy code

`> SADD myset1 "Hello"
> SADD myset1 "World"
> SADD myset2 "World"
> SDIFF myset1 myset2
["Hello"]`


### Sorted Set

Sorted sets in Redis are particularly useful when you want to maintain a list of items ordered by a scoring system. Here are a couple of real-world examples:

**Example 1: Leaderboard system in a game**

In an online gaming platform, you might want to maintain a leaderboard that keeps track of the top scores of each player. Since the score of each player can change dynamically, it becomes a challenge to maintain this leaderboard efficiently.

This is where a sorted set shines. In Redis, you can use a sorted set to represent the leaderboard, where each player's username is a member of the set, and the score is the score associated with that member.

Here is how you might use a sorted set to manage a leaderboard:

bashCopy code

`> ZADD leaderboard 5000 "player1"
> ZADD leaderboard 7500 "player2"
> ZADD leaderboard 6000 "player3"
> ZRANGE leaderboard 0 -1 WITHSCORES
["player1", "5000", "player3", "6000", "player2", "7500"]` 

In this example, the `ZADD` command is used to add members to the leaderboard. `ZRANGE` command is used to retrieve the leaderboard, sorted by score.

**Example 2: Time-series data**

Sorted sets are also useful when dealing with time-series data. For instance, in a system that logs events, you can use the timestamp of the event as the score and the event details as the member. This allows you to efficiently retrieve events based on their timestamp.

bashCopy code

`> ZADD eventlog 1631299200 "User1 logged in"
> ZADD eventlog 1631299260 "User2 posted a message"
> ZADD eventlog 1631299320 "User3 logged out"
> ZRANGE eventlog 0 -1 WITHSCORES
["User1 logged in", "1631299200", "User2 posted a message", "1631299260", "User3 logged out", "1631299320"]`


### Hahshes

##### 1. HSET

Sets field in the hash stored at key to value.

bashCopy code

`> HSET myhash field1 "Hello"
> HSET myhash field2 "World"` 

##### 2. HGET

Gets the value of a hash field.

bashCopy code

`> HSET myhash field1 "Hello"
> HGET myhash field1
"Hello"` 

##### 3. HGETALL

Gets all the fields and values in a hash.

bashCopy code

`> HSET myhash field1 "Hello"
> HSET myhash field2 "World"
> HGETALL myhash
["field1", "Hello", "field2", "World"]` 

##### 4. HDEL

Deletes one or more hash fields.

bashCopy code

`> HSET myhash field1 "Hello"
> HSET myhash field2 "World"
> HDEL myhash field1
> HGETALL myhash
["field2", "World"]` 

##### 5. HEXISTS

Determines if a hash field exists.

bashCopy code

`> HSET myhash field1 "Hello"
> HEXISTS myhash field1
(integer) 1
> HEXISTS myhash field2
(integer) 0` 

##### 6. HLEN

Gets the number of fields in a hash.

bashCopy code

`> HSET myhash field1 "Hello"
> HSET myhash field2 "World"
> HLEN myhash
(integer) 2` 

##### 7. HMSET

Sets multiple hash fields to multiple values.

bashCopy code

`> HMSET myhash field1 "Hello" field2 "World"
> HGETALL myhash
["field1", "Hello", "field2", "World"]` 

##### 8. HMGET

Gets the values of all the given hash fields.

bashCopy code

`> HMSET myhash field1 "Hello" field2 "World"
> HMGET myhash field1 field2
["Hello", "World"]` 

These are some of the most commonly used commands in Redis for working with hashes. Hashes are essentially maps between string fields and string values, making them perfect to represent objects.


### Pub/Sub

Redis Pub/Sub (Publish/Subscribe) is a simple and powerful messaging system that's built into Redis. It enables sending messages between different parts of an application, or separate applications or services, without them needing to poll each other or know about each other's existence. Here are some basic commands related to Pub/Sub:

##### 1. PUBLISH

Publishes a message to a channel.

bashCopy code

`> PUBLISH mychannel "Hello, World!"` 

##### 2. SUBSCRIBE

Subscribes the client to the specified channels.

bashCopy code

`> SUBSCRIBE mychannel` 

##### 3. PSUBSCRIBE

Subscribes the client to one or more channels that match a given pattern.

bashCopy code

`> PSUBSCRIBE my*` 

##### 4. UNSUBSCRIBE

Unsubscribes the client from the given channels, or from all of them if none is given.

bashCopy code

`> UNSUBSCRIBE mychannel` 

##### 5. PUNSUBSCRIBE 

Unsubscribes the client from one or more channels that match the given patterns, or from all if none is given.

bashCopy code

`> PUNSUBSCRIBE my*` 

Now let's assume that we have two Redis clients, a publisher and a subscriber. The subscriber wants to receive messages from the 'news' channel.

In the subscriber client:

bashCopy code

`> SUBSCRIBE news
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "news"
3) (integer) 1` 

In the publisher client:

bashCopy code

`> PUBLISH news "Hello, World!"
(integer) 1` 

Back in the subscriber client:

bashCopy code

`1) "message"
2) "news"
3) "Hello, World!"` 

As you see, the subscriber received the message published on the 'news' channel by the publisher.

#### - Real example of use Redis

 Very simple example of using Redis Pub/Sub in a Ruby application. 


**Publish.rb**

```ruby
`require 'redis'

redis = Redis.new(host: "localhost", port: 6379)

loop do
  message = gets.chomp
  redis.publish('my_channel', message)
end` 
```
This code creates a new Redis client connected to the server running on localhost and port 6379. The publisher reads messages from the command line and publishes them to 'my_channel'.

**Subscribe.rb**

```ruby
`require 'redis'

redis = Redis.new(host: "localhost", port: 6379)

redis.subscribe('my_channel') do |on| 
  on.message do |channel, message| 
    puts "Received '#{message}' on channel '#{channel}'"  
  end  
end
```

The subscriber creates another Redis client and subscribes to 'my_channel'. Whenever a message is received, it is printed to the console.

To test this, you would open two separate terminal windows. Run `ruby subscribe.rb` in the first one, which will start the subscriber. Then run `ruby publish.rb` in the second one, which will start the publisher. Now, whatever you type into the publisher terminal window will be received and printed by the subscriber.

Remember, both publisher and subscriber need access to the same Redis instance. The host and port specified when creating the Redis client should point to this instance. In these examples, it's assumed that Redis is running on localhost (the same machine) on port 6379, which is the default.

####  Kafka Prod/Consumer VS Redis Pub/Subs

Redis Pub/Sub and Apache Kafka are both messaging systems, but they serve different use cases and have different design principles.

**Redis Pub/Sub** is a lightweight messaging system that is built into Redis. It supports the publish-subscribe pattern where publishers send messages to channels without knowledge of what (if any) subscribers there may be. Subscribers express interest in one or more channels, and only receive messages that are of interest, without knowledge of what publishers there are.

A common use case for Redis Pub/Sub is to decouple the components of a monolithic application, turning it into a set of microservices that communicate with each other via messages. For example, in a web application, one service could publish a message whenever a new user signs up, and another service could listen for these messages and send a welcome email.

**Apache Kafka**, on the other hand, is a distributed streaming platform. It is designed to handle real-time data feeds with high throughput and distributed storage. Kafka maintains feeds of messages in categories called topics. Producers write data to topics and consumers read from topics. Kafka can also be used for decoupling services, just like Redis Pub/Sub, but Kafka is designed to handle a larger volume of messages and provide stronger durability guarantees.

Compared to Redis Pub/Sub, Kafka has several unique features:

1.  **Persistence**: Kafka stores all messages for a set amount of time, regardless of whether or not they have been consumed, allowing you to rewind and re-read messages.
    
2.  **Fault-tolerance**: Kafka replicates messages across multiple nodes to ensure that they aren't lost.
    
3.  **Ordering Guarantees**: Kafka guarantees that messages are read in the order they were written.
    
4.  **Consumer Groups**: Kafka supports the notion of consumer groups, which allows a pool of consumers to divide up the work of processing messages from one or more topics.
    
5.  **Recorded Offsets**: Kafka remembers the offset of the last record each consumer group has read.
    
6.  **Throughput**: Kafka is designed to handle high volumes of messages.
    

Whether to use Redis Pub/Sub or Apache Kafka depends on the specific requirements of your application. For simple, low-volume messaging between services in an application, Redis Pub/Sub is a good choice. For high-volume, fault-tolerant, ordered, and distributed data streaming, Apache Kafka is a better fit.


**Use Cases for Redis Pub/Sub:**

1.  **Real-time Notifications:** You could use Redis Pub/Sub to implement a real-time notification system where one part of your application publishes events, and other parts of the application subscribe to those events and take action when they occur.
    
2.  **Chat or Messaging Systems:** Redis Pub/Sub could be used to build a simple real-time chat or messaging system where each chat room or conversation is a separate channel.
    
3.  **Decoupling Services:** In a microservices architecture, Redis Pub/Sub can be used to decouple services by having them communicate with each other via messages instead of direct API calls. This can help to make the system as a whole more resilient and scalable.
    
4.  **Real-time Analytics:** You can use Redis Pub/Sub to collect and process real-time analytics, for example by publishing events every time a user visits a page or clicks on a link, and having a separate part of the application subscribe to these events and update the analytics in real time.
    

**Pros of Redis Pub/Sub:**

1.  **Simple and Easy to Use:** Redis Pub/Sub is easy to understand and use. It's built into Redis, so if you're already using Redis, there's nothing extra to install or configure.
    
2.  **Real-time:** Redis Pub/Sub is designed for real-time applications, with very low latency between when a message is published and when it is received by the subscribers.
    
3.  **Scalable:** You can easily add more publishers or subscribers as your application grows.
    
4.  **Decoupling:** Pub/Sub allows for decoupling of the data provider (publisher) and data consumer (subscriber) components. This means changes to one should not directly impact the other.
    

**Cons of Redis Pub/Sub:**

1.  **Lack of Durability:** Redis Pub/Sub is not designed to be a durable message queue - if a message is published while a client is not subscribed to the channel, that message is lost.
    
2.  **No Message Persistence:** Messages in Redis Pub/Sub are transient. Once a message has been delivered, it disappears from the system. There's no way to fetch the history of messages.
    
3.  **No Consumer Groups:** Unlike Kafka, Redis Pub/Sub does not have the concept of consumer groups. If two clients are subscribed to a channel, a message published to that channel will be delivered to both clients. There's no way to load balance messages across multiple consumers.
    
4.  **No Guaranteed Delivery:** There is no acknowledgment from the consumer side in Redis Pub/Sub that the message has been processed. If a subscriber goes offline or fails to process a message, the message might be lost.
    

In conclusion, Redis Pub/Sub is a great choice for applications that need real-time functionality and simplicity. However, for more complex use cases that require durability, persistence, or guaranteed delivery of messages, you might need to look at more advanced messaging systems like RabbitMQ or Apache Kafka.

Redis provides support for server-side scripting through Lua. This allows you to run scripts from the client side, which are then executed on the server side, directly in the Redis instance.

Lua was chosen for its simplicity, its established reputation, and the existence of an interpreter written in ANSI C (the same language in which Redis is written).

Server-side scripting has a few advantages:

1.  **Reduce Network Overhead:** If you're performing multiple operations as part of one logical transaction, it's often more efficient to run a script on the server side than to execute multiple commands from the client side.
    
2.  **Atomic Operations:** Redis guarantees that a script is executed in an atomic way: no other script or Redis command will be executed while a script is being executed. This makes it easier to reason about concurrency.
    

### Redis Scripts


Redis provides support for server-side scripting through Lua. This allows you to run scripts from the client side, which are then executed on the server side, directly in the Redis instance.

Lua was chosen for its simplicity, its established reputation, and the existence of an interpreter written in ANSI C (the same language in which Redis is written).

Server-side scripting has a few advantages:

1.  **Reduce Network Overhead:** If you're performing multiple operations as part of one logical transaction, it's often more efficient to run a script on the server side than to execute multiple commands from the client side.
    
2.  **Atomic Operations:** Redis guarantees that a script is executed in an atomic way: no other script or Redis command will be executed while a script is being executed. This makes it easier to reason about concurrency.


Here is a simple example of a Lua script that gets the value of a key and then deletes the key:


`> EVAL "return redis.call('get', KEYS[1])" 1 mykey` 

This script does two things:

1.  `redis.call('get', KEYS[1])` gets the value of the key. `redis.call` is a Lua function that lets you run any Redis command.
2.  `return` returns this value to the client.

`EVAL` takes the script as its first argument. The second argument is the number of keys that the script will access, and any arguments after that are passed to the script. In this case, `1` is the number of keys, and `mykey` is the key that the script will access.

You can also use the `EVALSHA` command to run scripts. Instead of taking the script itself, `EVALSHA` takes the SHA1 hash of the script. Redis caches scripts in memory, so using `EVALSHA` can save bandwidth by not needing to send the script itself in every request.

Here is how you would use `EVALSHA`:



> SCRIPT LOAD "return redis.call('get', KEYS[1])"
"6dcd4ce23d88e2ee95838f0b724b0719c86db742"

> EVALSHA "6dcd4ce23d88e2ee95838f0b724b0719c86db742" 1 mykey` 

First, you use `SCRIPT LOAD` to load the script into Redis. This returns the SHA1 hash of the script. You then pass this hash to `EVALSHA` to run the script.

## Redis Connection & Security

Redis clients connect to the Redis server using TCP connections. By default, Redis listens on TCP port 6379, but it can be configured to listen on any port, or on a UNIX domain socket.

### Examples

#### Connection to Redis

A typical Redis connection from a client might look like this:

bashCopy code

`redis-cli -h localhost -p 6379` 

#### Selecting a Database in Redis

Redis supports multiple databases, which are identified by a numeric index, not by database names. By default, a client is connected to database 0. You can switch to a different database using the SELECT command:

bashCopy code

`SELECT 1` 

### Security

#### Network Level Security

One of the most effective ways to protect your Redis instance is by limiting network access to your Redis server. This is typically done with firewall rules:

bashCopy code

`iptables -A INPUT -i eth1 -p tcp --destination-port 6379 -j DROP` 

#### Redis AUTH command

Redis provides a simple password-based authentication mechanism. Here is how you enable it:

bashCopy code

`CONFIG SET requirepass "yourpassword"` 

Clients must then use the AUTH command to authenticate:

bashCopy code

`AUTH "yourpassword"` 

#### SSL/TLS

As of Redis 6.0, Redis natively supports SSL/TLS, allowing secure client-server communication. This needs to be configured in the Redis configuration file.

#### Access Control Lists (ACL)

Redis 6.0 also introduced Access Control Lists (ACLs). An example of setting a user with an ACL:

bashCopy code

`ACL SETUSER new-user on >password +get +set +select` 

This sets a user "new-user" with the password "password", and this user can only execute the `get`, `set`, and `select` commands.

## Redis Benchmark

Redis comes with its own benchmarking tool called `redis-benchmark`. This tool allows you to measure the performance of your Redis server by generating a load of commands and measuring the latency of the responses.

The `redis-benchmark` tool provides a number of options to customize the benchmark tests, such as the number of parallel connections, the total number of requests, the types of commands to test, and so on.

Here's a simple example of how to use `redis-benchmark`:

bashCopy code

`redis-benchmark -q -n 100000` 

In this example, `-q` option is used to reduce verbosity (only show query/sec values), and `-n 100000` specifies that a total of 100,000 requests are made by the benchmark.

The output could look something like this:

bashCopy code

`PING_INLINE: 47524.47 requests per second
PING_BULK: 47393.43 requests per second
SET: 46838.27 requests per second
GET: 48231.56 requests per second
INCR: 46948.25 requests per second
LPUSH: 47169.47 requests per second` 

Each line shows the number of requests per second that the Redis server was able to handle for a particular command.

## Redis Databases

Redis has support for multiple databases. A fresh Redis instance will have 16 databases available, but this number can be changed. Each database provides a completely separate keyspace.

By default, a client is connected to database 0. However, you can switch between databases using the SELECT command:

bashCopy code

`SELECT 1` 

You can also move keys between databases using the MOVE command:

bashCopy code

`MOVE key 1` 

In the example above, the key "key" is moved to database 1.

Be aware, however, that while Redis does provide these multiple databases, they exist in the same memory space and run in the same process. This is unlike in SQL databases, where different databases are isolated from each other. In Redis, if one database consumes all the memory, it will impact all other databases.

## References

Redis Course - [In-Memory Database Tutorial](https://www.youtube.com/watch?v=XCsS_NVAa1g&t=684s) 