# NoSQL DBMS

For decades the most popular type of database was a relational database. RDBs align with ACID principles and have a big community who constantly enhance the RDB systems. But RDBs also have disadvantages which can have a crucial impact nowadays. 

As we know RDBs are enforcing schemes for its tables and simple schema change can create a big impact on software development process. Moreover, in comparison with NoSQL alternatives, RDBs are harder to scale horizontally due to the way it stores the data, as well as the fact that most RDBS are designed to work on single server and the fact that running multiple instances of RDBS can be costly. 

# Relational Database and Big Data

The most straightforward way to describe Big Data is that it has a high velocity (terabytes of data per second), it has high volume and large variety. Thus, relational databases don’t suit well for storing such data. There are two main disadvantages of relational databases in context of working with big data:

1. Big Data has a large variety of formats, thus RDB tables with not flexible schema definition won’t suite at all.
2. Big Data has a high velocity of data, thus RDBs with limited scaling options won’t suite well here.

# Rise of NoSQL Alternatives

So what should we expect from the database in order for it to be able to handle data with high variety, velocity and volume. 

1. Ability to store data without any schema.
2. Ability to scale the database fast and reliably.
3. Ability to fetch the data quickly.

Different NoSQL alternatives arise in last decade trying to support these features. NoSQL databases only have two things in common: they are schema-less and easy to scale horizontally. Below are the main NoSQL databases internal structure definitions.

## Key-Value Store

These are simple hash tables of unique keys and respective values. The values have no schema and the entries can not be linked. Usually there is no way to aggregate or search on entries’ values. The values can store any binary blob of information. These kind of databases can be used for caching (popular example is Redis) and in situation when one needs to store schema-less data and be able to quickly read by key.

## Column Family Store

These kind of databases are similar to the relational databases in terms that there are tables with rows and columns, but the key difference is that the row can have any number of different columns. Memory isn’t wasted for null/undefined row columns. It is important to pre-define column families, so that each column can be attached to specific group of columns. These kind of databases can be used for a situation when you have standard RBS model but you still don’t want to waste place on null columns, and you have to support fast write operations. 

## Graph Databases

Graph database are more about relationships between the entities than the entities itself. The key difference in comparison with RDBs is that the relationship itself can store data. In graph databases we have nodes and relationships. This way we can interconnect multiple nodes together. The relationships can also be bi-directional. The another popular modeling approach is that you are defining the subject which predicates to an object. These kind of databases are suitable when you have many relationships in your domain and you want insight about the relationships rather than nodes. 

## Document Store

The document store databases store a group of fields which form a document and identify a unique entity. The documents are grouped in collections and collections are stored in databases. This kind of databases don’t impose normalization, provide real parent-child relationship but still support associations between entities. Data is usually stored in BSON format (binary JSON).

## Search Engines

The search engines are used to provide a quick search on large volume of documents. It support inverted index which keep track on which document includes the key word(s) and returns the result reliably. 

# MongoDB

MongoDB is the most popular implementation of document store database nowadays. Whatever is mentioned above is also applied to MongoDB. Here I will just provide additional information on how to scale database. But before we start important definitions to keep in mind:

**Cluster:** A collection of interconnected servers (aka psychical machines)
**Replica Set:** A group of 3 servers which work together to support data redundancy and fault tolerance
**Shard:** A **l**ogical unit that has its own replica set and encapsulates a subset of a data.
**Shard Key:** A single or compound key which is used to split data into chunks.
**Config Server:** A logical unit consisting of a replica set which is responsible to storing metadata about the cluster’s shards as well as for balancing workload of the shards.
**Mongod:** A mongo daemon which handles all upcoming CRUD operations as well as actual data storage.
**Mongos:** A mongo router which handles all upcoming requests from clients and routes to respective shards. 
**Mongosh:** Mongo shell, which is used to interact with mongod and mongos instances.

The dataset is split into shards using the shard key. Each shard is logical unit which consist of replica set. The replica set store the data dedicated for that shard. Config servers store metadata about the cluster and are logical units too consisting from a replica set for redundancy and fail tolerance. The config server specifically responsible for providing metadata about each shard and each shard’s data subset. Balancer which is a component of a config server is responsible for balancing the workload across the shards’ replica sets. 

There are three things that one needs to keep in mind when defining a shard key:

1. The shard key should have a high cardinality,  meaning that it should contain a wide range of unique values for an efficient splitting into shards.
2. The distribution of shard key values should be random enough so that in case of write operations all the workload isn’t directed into one shard.
3. Ideally queries should target a data which resides in one shard.

It isn’t possible to satisfy all three points but these should be considered to define a right shard key.