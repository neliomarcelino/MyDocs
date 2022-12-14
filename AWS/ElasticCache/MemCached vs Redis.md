## MemCached
---
> On the surface, the engines look similar. Each of them is an in-memory key-value store. However, in practice there are significant differences.

-   You need the simplest model possible.
-   You need to run large nodes with multiple cores or threads.
-   You need the ability to scale out and in, adding and removing nodes as demand on your system increases and decreases.
-   You need to cache objects.

## Redis
---
>Choose Redis with a version of ElastiCache for Redis if the following apply for you:

-   **ElastiCache for Redis version 6.2 (Enhanced)**
    You want the ability to tier data between memory and SSD using the r6gd node type. For more information, see [Data tiering](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/data-tiering.html).
    
-   **ElastiCache for Redis version 6.0 (Enhanced)**
    You want to authenticate users with role-based access control.
    For more information, see [Redis Version 6.0 (Enhanced)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-6.0).
    
-   **ElastiCache for Redis version 5.0.0 (Enhanced)**
    You want to use [Redis streams](https://redis.io/topics/streams-intro), a log data structure that allows producers to append new items in real time and also allows consumers to consume messages either in a blocking or non-blocking fashion.
    For more information, see [Redis Version 5.0.0 (Enhanced)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-5-0).
    
-   **ElastiCache for Redis version 4.0.10 (Enhanced)**
    Supports both encryption and dynamically adding or removing shards from your Redis (cluster mode enabled) cluster.
    For more information, see [Redis Version 4.0.10 (Enhanced)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-4-0-10).
    
-   **ElastiCache for Redis version 3.2.10 (Enhanced)**
	Does not support Encryption! -> looked at 2022-12-02
    Supports the ability to dynamically add or remove shards from your Redis (cluster mode enabled) cluster.
	For more information, see the following:
	-   [Redis Version 3.2.10 (Enhanced)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-3-2-10)
	-   Online resharding best practices for Redis, For more information, see the following:
	    -   [Best Practices: Online Resharding](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/best-practices-online-resharding.html)
	    -   [Online Resharding and Shard Rebalancing for Redis (Cluster Mode Enabled)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/redis-cluster-resharding-online.html)
	-   For more information on scaling Redis clusters, see [Scaling](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html).

-   **ElastiCache for Redis version 3.2.6 (Enhanced)**
    If you need the functionality of earlier Redis versions plus the following features, choose ElastiCache for Redis 3.2.6:
    -   In-transit encryption. For more information, see [Amazon ElastiCache for Redis In-Transit Encryption](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html).
    -   At-rest encryption. For more information, see [Amazon ElastiCache for Redis At-Rest Encryption](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html).
    -   HIPAA eligibility certification. For more information, see [HIPAA Eligibility for Amazon ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-compliance-hipaa.html).
    
-   **ElastiCache for Redis (Cluster mode enabled) version 3.2.4**
    If you need the functionality of Redis 2.8.x plus the following features, choose Redis 3.2.4 (clustered mode):
    -   You need to partition your data across two to 500 node groups (clustered mode only).
    -   You need geospatial indexing (clustered mode or non-clustered mode).
    -   You don't need to support multiple databases.
    
-   **ElastiCache for Redis (non-clustered mode) 2.8.x and 3.2.4 (Enhanced)**
    If the following apply for you, choose Redis 2.8.x or Redis 3.2.4 (non-clustered mode):
    -   You need complex data types, such as strings, hashes, lists, sets, sorted sets, and bitmaps.
    -   You need to sort or rank in-memory datasets.
    -   You need persistence of your key store.
    -   You need to replicate your data from the primary to one or more read replicas for read intensive applications.
    -   You need automatic failover if your primary node fails.
    -   You need publish and subscribe (pub/sub) capabilities—to inform clients about events on the server.
    -   You need backup and restore capabilities.
    -   You need to support multiple databases.


[More info at AWS DOCs](https://aws.amazon.com/elasticache/redis-vs-memcached/)
[More info at AWS whitepapers](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf)