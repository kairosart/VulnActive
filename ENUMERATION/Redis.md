**Redis** is an **open-source, in-memory data structure store** used as:

- a **database**

- a **cache**

- a **message broker**

- and sometimes a **streaming engine**


---

## 🔧 What Redis Actually Is

- **In-memory** → Stores data in RAM for very fast access

- **Key-Value Store** → Like a dictionary: you store data as `key: value`

- **Super fast** → Often used where low latency is critical (e.g. caching, real-time apps)


---

## 🧠 What You Can Do With Redis

Redis supports multiple **data types**, not just strings:

|Data Type|Example Use Case|
|---|---|
|**Strings**|Simple values or counters|
|**Lists**|Queues, logs|
|**Sets**|Unique tags, user IDs|
|**Hashes**|Representing objects (e.g., user profiles)|
|**Sorted Sets**|Leaderboards, priority queues|
|**Streams**|Event processing|
|**Bitmaps / HyperLogLogs**|Analytics (unique counts, etc.)|
## 🔐 Security Note

If misconfigured, Redis can be **dangerous** when exposed to the internet (e.g., via `bind 0.0.0.0` with no password). Always secure Redis in production.


---
## Enumeration

- Grab some date from Redis.

```
redis-cli -h <MACHINE IP>
10.10.252.25:6379> INFO
```

![[Screenshot_2025-10-04_06-17-04.png]]

![[Screenshot_2025-10-04_06-21-01.png]]

Notice the Redis version is very old (*2.8.2402,* currently 6.2.6).

- Check the config
```
10.10.252.25:6379> CONFIG GET *
```

![[Screenshot_2025-10-04_06-26-26.png]]

The current user name is `enterprise-security`.

**Next step:** [[SMB credentials capturing]]
