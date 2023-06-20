### Replica set

A replica set is a core component of MongoDB, which is a popular NoSQL database system. It provides high availability and data redundancy by maintaining multiple copies of data across multiple servers, known as replicas. A replica set consists of multiple MongoDB instances, where one instance acts as the primary and the others serve as secondary replicas.<br>

The primary replica is responsible for all write operations and becomes the authoritative source of data. The secondary replicas replicate data from the primary and can serve read operations as well. The primary replica continuously replicates data changes to the secondary replicas, ensuring that they stay up to date.<br>

Replica sets offer automatic failover, meaning that if the primary replica becomes unavailable due to a failure or planned maintenance, one of the secondary replicas automatically steps up to become the new primary. This process ensures that the system remains operational even in the presence of failures.


## Sharding

Horizontal scaling 
 each shard is a replica set

 there is a middle man in this casea to perform the process called router
 the job of router is to forward the request to the respective re