# Redis-GO

**Redis-GO** is a high-performance, multi-threaded Redis clone written in Go. It implements the core Redis protocol (RESP) and features a variety of data structures, I/O multiplexing for concurrency, and advanced memory management strategies like LRU eviction.

## Features

* **Core Architecture**:
    * **I/O Multiplexing**: Uses `epoll` on Linux and `kqueue` on macOS for efficient non-blocking I/O.
    * **Concurrency Models**: Supports both a single-threaded event loop and a multi-threaded Reactor pattern with dedicated I/O handlers and worker pools.
    * **RESP Protocol**: Full implementation of the Redis Serialization Protocol (RESP) for compatibility with standard Redis clients.
* **Data Structures**:
    * **Strings**: Basic key-value storage with expiration support.
    * **Sets**: Unordered collections of unique strings.
    * **Sorted Sets**: Implemented using Skiplists and Hash Maps for efficient ranking and scoring.
    * **Probabilistic Data Structures**:
        * **Bloom Filters**: Space-efficient probabilistic data structure to test set membership.
        * **Count-Min Sketch**: Probabilistic data structure for frequency estimation of events.
* **Memory Management**:
    * **Expiration**: Implements both **lazy expiration** (on access) and **active expiration** (random sampling) strategies.
    * **Eviction Policies**: Supports `allkeys-lru` and `allkeys-random` to free up memory when the limit is reached.


## Supported Commands

### Key-Value & General
* `PING`: Check server health.
* `SET key value [TTL]`: Set a key with an optional Time-To-Live (in seconds).
* `GET key`: Retrieve the value of a key.
* `TTL key`: Get the remaining time to live for a key.
* `INFO`: Get server statistics (e.g., number of keys).

### Sets (`S`)
* `SADD key member [member ...]`: Add members to a set.
* `SREM key member [member ...]`: Remove members from a set.
* `SMEMBERS key`: Get all members of a set.
* `SISMEMBER key member`: Check if a member exists in a set.

### Sorted Sets (`Z`)
* `ZADD key score member [score member ...]`: Add members with scores to a sorted set.
* `ZSCORE key member`: Get the score of a member.
* `ZRANK key member`: Get the rank of a member (0-based).

### Bloom Filters (`BF`)
* `BF.RESERVE key error_rate capacity`: Create a new Bloom filter.
* `BF.MADD key item [item ...]`: Add items to the Bloom filter.
* `BF.EXISTS key item`: Check if an item exists in the Bloom filter.

### Count-Min Sketch (`CMS`)
* `CMS.INITBYDIM key width depth`: Initialize CMS by dimensions.
* `CMS.INITBYPROB key error_rate probability`: Initialize CMS by error probability.
* `CMS.INCRBY key item increment [item increment ...]`: Increment the count for items.
* `CMS.QUERY key item [item ...]`: Query the estimated count for items.