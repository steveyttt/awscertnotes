Metrics to monitor
- CPU
- swap
- eviction
- concurrent connections

memcacheD - If CPU is above 90% add more nodes to the cluster

swap - should only ever be 50Mb - aim for zero
with memcacheD you don't want to be using your swap file
- If overhead pool (pageing) is less than 50MB (so barely used) adjusting memory overhead wont help. 
- - Instead look into connection counts.

What is eviction?
- When old data is purged to make way for new data.

With MemcacheD - if you hit limits, scale out or UP
With Redis - can only scape out (Add more read replicas)

You can monitor for concurrent connections on both to see if you have a surge in traffic OR if applications are not releasing connections. Set an alarm here as needed.

MEMcacheD DOES NOT support multi AZ!!!!!
Redis does support multiAZ and master slave / primary secondary replication....

MemcacheD does not support encryption.
Redis does however. (In transit and at rest) (Some conditions apply with at rest)


in exam questions 
- this can come up if your RDS instances is under pressure. Elasticache can solve this if your instance is read heavy and rarely changes.
- if instance is stressed becuase of OLAP (Online analytics proceessing) though think redshift

can provide:
- sorted sets
- pub/sub
- in-memory data store

