# RedisCache
A comprehensive guide to learning Redis
# Redis Learning Guide

A comprehensive guide to mastering Redis, from basic concepts to advanced techniques.

## Table of Contents
- [What is Redis?](#what-is-redis)
- [Installation](#installation)
- [Learning Path](#learning-path)
- [Core Concepts](#core-concepts)
- [Data Types](#data-types)
- [Commands Reference](#commands-reference)
- [Advanced Features](#advanced-features)
- [Performance and Optimization](#performance-and-optimization)
- [Production Considerations](#production-considerations)
- [Client Libraries](#client-libraries)
- [Hands-On Projects](#hands-on-projects)
- [Resources](#resources)

## What is Redis?

**Redis** (Remote Dictionary Server) is an open-source, in-memory data structure store used as a database, cache, and message broker. It supports various data structures and is known for its exceptional performance.

### Key Characteristics:
- **In-memory storage** - All data resides in RAM for ultra-fast access
- **Persistent** - Data can be saved to disk for durability
- **Atomic operations** - All operations are atomic by nature
- **Support for complex data types** - Beyond simple key-value pairs
- **High availability** - Built-in replication and clustering
- **Pub/Sub messaging** - Real-time messaging capabilities

### Common Use Cases:
- **Caching** - Session storage, page caching, API response caching
- **Real-time analytics** - Leaderboards, counters, metrics
- **Message queues** - Task queues, pub/sub systems
- **Session management** - User session data
- **Rate limiting** - API rate limiting, request throttling
- **Geospatial applications** - Location-based services

## Installation

### Local Installation

#### macOS (using Homebrew):
```bash
brew install redis
redis-server
```

#### Ubuntu/Debian:
```bash
sudo apt update
sudo apt install redis-server
sudo systemctl start redis-server
```

#### Windows:
- Use WSL2 or download Redis for Windows
- Or use Docker (recommended)

#### Docker:
```bash
# Pull and run Redis
docker run -d -p 6379:6379 --name redis redis:latest

# Connect to Redis CLI
docker exec -it redis redis-cli
```

### Cloud Options:
- **Redis Cloud** - Managed Redis by Redis Labs
- **AWS ElastiCache** - Amazon's managed Redis
- **Azure Cache for Redis** - Microsoft's managed Redis
- **Google Cloud Memorystore** - Google's managed Redis

## Learning Path

### Phase 1: Fundamentals (Week 1-2)
1. Understand Redis basics and installation
2. Learn Redis CLI and basic commands
3. Master string operations
4. Practice with Redis data types

### Phase 2: Data Structures (Week 3-4)
1. Deep dive into all Redis data types
2. Learn when to use each data structure
3. Practice complex operations
4. Understand time complexity

### Phase 3: Advanced Operations (Week 5-6)
1. Transactions and pipelines
2. Lua scripting
3. Pub/Sub messaging
4. Persistence mechanisms

### Phase 4: Production Ready (Week 7-8)
1. Security and authentication
2. Performance optimization
3. Monitoring and debugging
4. High availability setup

### Phase 5: Mastery (Week 9-12)
1. Redis Cluster
2. Custom modules
3. Advanced patterns
4. Real-world projects

## Core Concepts

### Memory Model
- All data stored in RAM for fast access
- Optional persistence to disk
- Memory is the limiting factor

### Single-threaded Nature
- Uses single-threaded event loop
- No race conditions
- High concurrency through multiplexing

### Atomic Operations
- All operations are atomic
- No partial updates
- ACID properties for transactions

### Key Expiration
- TTL (Time To Live) support
- Automatic cleanup of expired keys
- Memory management

## Data Types

### 1. Strings
The most basic Redis data type, binary-safe strings.

```bash
# Set and get
SET mykey "Hello World"
GET mykey

# Increment operations
SET counter 10
INCR counter          # 11
INCRBY counter 5      # 16
DECR counter          # 15

# Append operations
APPEND mykey " Redis"  # "Hello World Redis"

# Multiple operations
MSET key1 "value1" key2 "value2"
MGET key1 key2
```

### 2. Lists
Ordered collections of strings, implemented as linked lists.

```bash
# Push operations
LPUSH mylist "first"   # Add to beginning
RPUSH mylist "last"    # Add to end

# Pop operations
LPOP mylist           # Remove from beginning
RPOP mylist          # Remove from end

# Range operations
LRANGE mylist 0 -1    # Get all elements
LLEN mylist          # Get list length

# List as queue (FIFO)
LPUSH queue "job1"
LPUSH queue "job2"
RPOP queue           # Returns "job1"

# List as stack (LIFO)
LPUSH stack "item1"
LPUSH stack "item2"
LPOP stack          # Returns "item2"
```

### 3. Sets
Unordered collections of unique strings.

```bash
# Add members
SADD myset "member1" "member2" "member3"

# Check membership
SISMEMBER myset "member1"  # 1 (true)

# Set operations
SADD set1 "a" "b" "c"
SADD set2 "b" "c" "d"
SINTER set1 set2         # Intersection: "b", "c"
SUNION set1 set2         # Union: "a", "b", "c", "d"
SDIFF set1 set2          # Difference: "a"

# Random operations
SRANDMEMBER myset        # Get random member
SPOP myset              # Remove and return random member
```

### 4. Hashes
Maps between string fields and string values.

```bash
# Set hash fields
HSET user:1 name "John Doe" email "john@example.com" age "30"

# Get hash fields
HGET user:1 name         # "John Doe"
HMGET user:1 name email  # Multiple fields
HGETALL user:1          # All fields and values

# Hash operations
HINCRBY user:1 age 1     # Increment age
HEXISTS user:1 phone    # Check if field exists
HDEL user:1 age         # Delete field
```

### 5. Sorted Sets (ZSets)
Ordered sets where each member has an associated score.

```bash
# Add members with scores
ZADD leaderboard 100 "player1" 200 "player2" 150 "player3"

# Range operations
ZRANGE leaderboard 0 -1           # By rank (ascending)
ZREVRANGE leaderboard 0 -1        # By rank (descending)
ZRANGEBYSCORE leaderboard 100 200 # By score range

# Score operations
ZSCORE leaderboard "player1"      # Get score
ZINCRBY leaderboard 50 "player1"  # Increment score

# Rank operations
ZRANK leaderboard "player1"       # Get rank (0-based)
ZREVRANK leaderboard "player1"    # Get reverse rank
```

### 6. Bitmaps
String data type with bit-level operations.

```bash
# Set bits
SETBIT users:2023-01-01 123 1  # User 123 was active
SETBIT users:2023-01-01 456 1  # User 456 was active

# Get bits
GETBIT users:2023-01-01 123    # Check if user 123 was active

# Count bits
BITCOUNT users:2023-01-01      # Count active users

# Bitwise operations
BITOP AND result users:2023-01-01 users:2023-01-02
```

### 7. HyperLogLog
Probabilistic data structure for cardinality estimation.

```bash
# Add elements
PFADD visitors "user1" "user2" "user3"

# Count unique elements
PFCOUNT visitors               # Approximate count

# Merge HyperLogLogs
PFMERGE total_visitors monthly_visitors weekly_visitors
```

### 8. Geospatial
Geographic coordinates and radius queries.

```bash
# Add locations
GEOADD locations 2.349014 48.864716 "Eiffel Tower"
GEOADD locations 2.295226 48.873792 "Arc de Triomphe"

# Calculate distance
GEODIST locations "Eiffel Tower" "Arc de Triomphe" km

# Find nearby locations
GEORADIUS locations 2.349014 48.864716 5 km
```

## Commands Reference

### Connection Commands
```bash
PING                    # Test connection
AUTH password          # Authenticate
SELECT 0              # Select database
QUIT                  # Close connection
```

### Key Management
```bash
EXISTS key            # Check if key exists
DEL key              # Delete key
EXPIRE key 60        # Set expiration (seconds)
TTL key             # Get time to live
PERSIST key         # Remove expiration
KEYS pattern        # Find keys (avoid in production)
SCAN 0 MATCH pattern # Iterate keys safely
TYPE key           # Get key type
```

### Server Commands
```bash
INFO               # Server information
DBSIZE            # Number of keys
FLUSHDB          # Clear current database
FLUSHALL         # Clear all databases
CONFIG GET *     # Get configuration
MONITOR          # Monitor commands (debug only)
```

## Advanced Features

### Transactions
Redis transactions allow executing multiple commands atomically.

```bash
# Basic transaction
MULTI
SET key1 "value1"
SET key2 "value2"
EXEC

# With watch (optimistic locking)
WATCH key1
MULTI
SET key1 "new_value"
EXEC
```

### Pipelining
Send multiple commands without waiting for replies.

```python
import redis

r = redis.Redis()

# Without pipelining (slow)
for i in range(1000):
    r.set(f"key{i}", f"value{i}")

# With pipelining (fast)
pipe = r.pipeline()
for i in range(1000):
    pipe.set(f"key{i}", f"value{i}")
pipe.execute()
```

### Lua Scripting
Execute Lua scripts atomically on the Redis server.

```lua
-- Rate limiting script
local key = KEYS[1]
local limit = tonumber(ARGV[1])
local window = tonumber(ARGV[2])

local current = redis.call('GET', key)
if current == false then
    redis.call('SET', key, 1)
    redis.call('EXPIRE', key, window)
    return 1
elseif tonumber(current) < limit then
    return redis.call('INCR', key)
else
    return 0
end
```

```bash
# Save script and execute
redis-cli SCRIPT LOAD "$(cat rate_limit.lua)"
# Returns: "sha1_hash"

# Execute script
EVALSHA sha1_hash 1 "api:user:123" 10 60
```

### Pub/Sub Messaging
Real-time messaging between clients.

```bash
# Subscriber
SUBSCRIBE news weather sports

# Publisher
PUBLISH news "Breaking news: Redis 7.0 released!"
PUBLISH weather "Sunny, 25°C"

# Pattern subscription
PSUBSCRIBE news.*
PUBLISH news.tech "New tech announcement"
```

### Streams
Log-like data structure for real-time data.

```bash
# Add to stream
XADD mystream * sensor 123 temperature 19.8

# Read from stream
XREAD COUNT 2 STREAMS mystream 0
XRANGE mystream - +

# Consumer groups
XGROUP CREATE mystream group1 0
XREADGROUP GROUP group1 consumer1 STREAMS mystream >
```

## Performance and Optimization

### Memory Optimization
1. **Choose appropriate data types**
   - Use hashes for objects instead of separate keys
   - Use bit operations for boolean flags
   - Consider HyperLogLog for large cardinality

2. **Key naming conventions**
   - Use consistent, descriptive naming
   - Avoid very long key names
   - Use separators like colons: `user:123:profile`

3. **Expiration policies**
   - Set TTL for temporary data
   - Use `EXPIRE` and `EXPIREAT` appropriately
   - Monitor memory usage

### Performance Tips
1. **Use pipelining** for multiple operations
2. **Avoid KEYS command** in production (use SCAN)
3. **Use Lua scripts** for complex atomic operations
4. **Monitor slow commands** with SLOWLOG
5. **Configure appropriate timeouts**

### Memory Analysis
```bash
# Memory usage
INFO memory
MEMORY USAGE keyname

# Key sampling
MEMORY DOCTOR
DEBUG OBJECT keyname
```

## Production Considerations

### Security
```bash
# Authentication
requirepass your_strong_password

# Rename dangerous commands
rename-command FLUSHALL ""
rename-command CONFIG "CONFIG_HIDDEN"

# Network security
bind 127.0.0.1
port 0  # Disable standard port
unixsocket /tmp/redis.sock
```

### Persistence Options

#### RDB (Redis Database)
- Point-in-time snapshots
- Compact, good for backups
- Faster restart times

```bash
# Configuration
save 900 1      # Save if 1 key changes in 900 seconds
save 300 10     # Save if 10 keys change in 300 seconds
save 60 10000   # Save if 10000 keys change in 60 seconds
```

#### AOF (Append Only File)
- Log of all write operations
- Better durability
- Larger file size

```bash
# Configuration
appendonly yes
appendfsync everysec  # always, everysec, or no
```

### High Availability

#### Replication
```bash
# Master-slave setup
# On slave:
SLAVEOF master_host master_port

# Configuration
repl-backlog-size 1mb
repl-timeout 60
```

#### Sentinel
Automatic failover and monitoring.

```bash
# sentinel.conf
sentinel monitor mymaster 127.0.0.1 6379 2
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 10000
```

#### Cluster
Horizontal scaling with sharding.

```bash
# Cluster setup (minimum 6 nodes)
redis-server --port 7000 --cluster-enabled yes
redis-server --port 7001 --cluster-enabled yes
# ... more nodes

# Create cluster
redis-cli --cluster create 127.0.0.1:7000 127.0.0.1:7001 ...
```

### Monitoring
```bash
# Key metrics to monitor
INFO stats
INFO replication
INFO memory
INFO clients

# Slow log
SLOWLOG GET 10
CONFIG SET slowlog-log-slower-than 10000

# Client connections
CLIENT LIST
CLIENT KILL ip:port
```

## Client Libraries

### Popular Libraries by Language

#### Python
```python
import redis

# Basic connection
r = redis.Redis(host='localhost', port=6379, db=0)

# Connection pool
pool = redis.ConnectionPool(host='localhost', port=6379, db=0)
r = redis.Redis(connection_pool=pool)

# Cluster
from rediscluster import RedisCluster
startup_nodes = [{"host": "127.0.0.1", "port": "7000"}]
r = RedisCluster(startup_nodes=startup_nodes)
```

#### Node.js
```javascript
const redis = require('redis');

// Basic connection
const client = redis.createClient();

// With options
const client = redis.createClient({
  host: 'localhost',
  port: 6379,
  retry_strategy: (options) => Math.min(options.attempt * 100, 3000)
});

// Promises (ioredis)
const Redis = require('ioredis');
const redis = new Redis();
```

#### Java
```java
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;

// Basic connection
Jedis jedis = new Jedis("localhost", 6379);

// Connection pool
JedisPool pool = new JedisPool("localhost", 6379);
try (Jedis jedis = pool.getResource()) {
    jedis.set("key", "value");
}
```

#### Go
```go
import "github.com/go-redis/redis/v8"

rdb := redis.NewClient(&redis.Options{
    Addr:     "localhost:6379",
    Password: "",
    DB:       0,
})

ctx := context.Background()
err := rdb.Set(ctx, "key", "value", 0).Err()
```

## Hands-On Projects

### Project 1: Simple Cache
Build a web application cache layer.

**Requirements:**
- Cache API responses
- Implement TTL for cache entries
- Cache invalidation strategies

### Project 2: Real-time Leaderboard
Create a gaming leaderboard system.

**Requirements:**
- Player score tracking
- Top N players queries
- Real-time updates

### Project 3: Rate Limiter
Implement an API rate limiting service.

**Requirements:**
- Per-user rate limits
- Different time windows
- Sliding window algorithm

### Project 4: Session Store
Build a session management system.

**Requirements:**
- User session storage
- Session expiration
- Concurrent session handling

### Project 5: Chat Application
Real-time messaging with Redis Pub/Sub.

**Requirements:**
- Channel-based messaging
- User presence tracking
- Message history

### Project 6: Analytics Dashboard
Real-time analytics with Redis data structures.

**Requirements:**
- Event counting with HyperLogLog
- Time-series data with sorted sets
- Geographic data analysis

## Resources

### Official Documentation
- [Redis Documentation](https://redis.io/documentation)
- [Redis Commands](https://redis.io/commands)
- [Redis University](https://university.redis.com/)

### Books
- "Redis in Action" by Josiah L. Carlson
- "Redis Essentials" by Maxwell Da Silva
- "Mastering Redis" by Jeremy Nelson

### Online Courses
- Redis University (Free courses)
- Udemy Redis courses
- Pluralsight Redis path

### Tools and GUIs
- **RedisInsight** - Official GUI tool
- **Redis Commander** - Web-based Redis management
- **Medis** - macOS Redis GUI
- **Redis Desktop Manager** - Cross-platform GUI

### Community
- [Redis GitHub](https://github.com/redis/redis)
- [Redis Discord](https://discord.gg/redis)
- [Redis Subreddit](https://reddit.com/r/redis)
- [Stack Overflow Redis tag](https://stackoverflow.com/questions/tagged/redis)

### Conferences and Events
- RedisConf (Annual conference)
- Redis meetups worldwide
- Cloud provider Redis sessions

## Practice Exercises

### Beginner Level
1. Set up Redis and connect via CLI
2. Practice all basic data type operations
3. Implement a simple counter
4. Create a basic pub/sub system
5. Set up key expiration

### Intermediate Level
1. Build a caching layer for a web app
2. Implement a distributed lock
3. Create a simple queue system
4. Use Lua scripts for atomic operations
5. Set up master-slave replication

### Advanced Level
1. Configure Redis Cluster
2. Implement complex data pipelines
3. Build a real-time analytics system
4. Create custom Redis modules
5. Optimize Redis for specific use cases

## Next Steps
1. Choose a project that interests you
2. Start with basic Redis installation and CLI
3. Practice with different data types daily
4. Build progressively complex applications
5. Join the Redis community for support
6. Consider Redis certification

Remember: Redis is best learned through hands-on practice. Start simple, build projects, and gradually tackle more complex scenarios. The key is consistent practice and real-world application of concepts.

---

*Happy Redis learning! 🚀*
