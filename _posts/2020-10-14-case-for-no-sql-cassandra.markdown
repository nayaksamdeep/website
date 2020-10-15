---
layout: post
title:  "Case for NoSQL - Cassandra Overview"
date:   2020-10-14 00:36:19 +0530
categories: architecture
---
With the explosion of consumer oriented applications in the last decade,
we are seeing immense pressure on data handling by the Volume, Velocity
and Variety. A large number of these applications tend to be read heavy
and do not require strong ACID properties. All of this has given rise to
non-relational databases that provide a means to store and retrieval of
data at scale. The horizontal scaling and usage of commodity hardware is
another key attribute of these solutions. The horizontal scaling tail
wind has started giving rise to a new generation of RDBMS that can
scale(such as
[[voltdb]{.ul}](http://www.odbms.org/wp-content/uploads/2013/11/VoltDBTechnicalOverview.pdf)),
but in this blog we will try to cover the architecture of NoSQL database
and Cassandra in specific. 

*1.0 Types of NoSQL*
--------------------

**1.1 Key/Value Based**
========================

The data is stored in Key Value Pairs. They are primarily used for
storing details of the customer, memory caching etc. Example
implementations include DynamoDB, Redis etc

**1.2 Wide Row**
==================

These databases store data in rows and the users are able to do the
query using the column based access. They are primarily used for high
volume/velocity storing time series data, geographical information,
sensor data etc. Example implementations include Cassandra, HBase

**1.3  Columnar**
====================

Columnar databases are optimised to fast retrieve columnar data
typically for analytical applications. They are primarily used for Data
WareHouse and Big Data Processing. Example implementations include
Amazon Redshift and Google BigQuery

**1.4  Document**
===================

A document-oriented database is a modern way to store data in JSON
format rather than simple rows and columns. These are designed for
storing, retrieving, and managing document-oriented information, also
known as semi-structured data. They are used in storing product
catalogue, content management systems, storing user profiles etc.
Example implementations include MongoDB and CouchDB

**1.5  Graph**
================

Graph databases are built to store and navigate relationships. They are
used in Fraud detection, recommendation engine etc, Example
implementations include Neo4j, OrientDB, TitanDB

*2.0 Recommended Read Before You Start *
--------------------------------------------

I highly recommend you go through this blog by Sandeep Uttamchandani on
the Blue Print of a Distributed Storage System
[[here]{.ul}](https://medium.com/@sandeepu/under-the-hood-of-a-distributed-storage-system-4a1a2c0b771d)
This covers the key concepts of distributed systems

Here are some of the key concepts that you may want to remember

**2.1 Clustering techniques**
==============================

Clustering model defines the approach by which the individual nodes
within a scale-out cluster coordinate to manage resources, handle
events, and support storage data and metadata operations for the
clients.

**2.1.1 Master Taxonomy**

In a cluster there is a super node that is responsible for managing the
cluster state, determine data placement and co-ordinate activities. HDFS
is a good example.

**2.1.2 Masterless Taxonomy**

All the nodes in a cluster share equal responsibilities for managing the
cluster state and activities. Cassandra, redis are great example

**2.1.3 Multi-Master Taxonomy**

Here the Master acts as the light-weight coordinator, and divides the
cluster management tasks among the among the nodes within the cluster.
Ceph and Mongodb are great examples

**2.2 Dataset Partitioning**
=============================

With horizontal scaling, the data is spread amongst multiple nodes.
There are different ways to address them. Some of the key ones are
captured below

**2.2.1 Range Based**

The key-space is divided into ranges and assigned to individual nodes.
The nodes can be physical or a Virtual Machine. Optionally a physical
system can hostt multiple nodes called virtual nodes that would be
allocated with their own ranges. Consistent hashing is typically used
and this is popular with Masterless architectures

**2.2.2 Directory Based**

A centralised directory tracks the mapping of name space to nodes. 

**2.3 Mutability**
====================

Mutability model defines the strategy to update existing data. 

**2.3.1 In-Place**

Updates are written directly in existing data locations are modified.

**2.3.2 Out-Of-Place**

Updates to the namespace object are persisted at a new resource location
(i.e., logical or physical address). The metadata associated with the
namespace object is updated to reflect the change.

**2.3.3 Versioning**

The update is persisted as a new version of the object.

*3.0 Cassandra Overview*
-------------------------------

Cassandra follows a masterless architecture along with range based data
placement. It is location sensitive and can place the replicas in
different racks or datacentres to increase the availability. Here are
the key components of Cassandra

**3.1 Key Infrastructure Components**
======================================

**3.1.1 Node**

This is the basic building block of a Cassandra cluster. This is where
the data is stored. A Virtual Machine or a Physical Server can act as a
Node. You can further split a node into multiple virtual nodes. This
helps reduce the partition range that each node/virtual node is
responsible for and helps with extending/compacting a cluster

**3.1.2 Rack**

A rack in the Data Centre world consists of several servers which are
supported by the same source of power, networking and real estate.
Storing multiple replicas on the same rack is risky and Cassandra can
place the replicas on different racks (when it is available)

**3.1.3 Datacentre**

A datacentre consists of multiple racks. A datacentre typically does not
span multiple locations and the network latency is low for the east-west
traffic. Both Rack and Datacentre can also be virtual constructs in case
Cassandra as long as the key concepts are met 

**3.1.4 Cluster**

A cluster typically spans multiple datacentres. This is a virtual
construct. 

**3.1.5 Snitch**

Cassandra does not detect the racks and datacentres automatically and
will rely on user inputs. So if you started thinking about how Cassandra
goes about determining the network topology, you do not have to dive
deeper into networking now. Snitch in Cassandra helps determine which
node should get the replica based on user inputs as well as node health.
It has a dynamic layer which looks at the performance of nodes and
recommends nodes for reading.

**3.2 Key Components of a Node**
=================================

A node stores the data that is directed to it. The data can be Primary
data or a Replica Copy. The data is committed in 2 phases. The need for
sucn 2 phase commit may have to be revisited with the evolution of
Persistent Memory in the market.

**3.2.1 Commit Log**

The record is first written into local disk. The assumption is that the
record is a small unit compared to a sector of hard disk.

**3.2.2 Mem Table**

As the name suggests, the mem table is all in memory and the incoming
record is appended to the table. When the table is full, it is flushed
to the disk. Alternatively a user can force a flush or we can flush the
data after the timeout value

**3.2.3 SSTable**

A sorted string table (SSTable) is an immutable data file to which
Cassandra writes memtables periodically. SSTables are append only and
stored on disk sequentially and maintained for each Cassandra table.
After the data is written to the SSTable, Commit Logs can be deleted.

**3.3 Key Components of a Cluster**
====================================

Each Cassandra cluster has a unique name. All the participating nodes
for that cluster use the name to join. 

**3.3.1 Seed Node and Boot Strapping**

When a node starts up it looks to its seed node to obtain information
about the other nodes in the cluster. E.g When a second node is added to
the cluster, it will have the first node in the cluster as the seed
node. This helps bootstrap the second node. A seed node can be any node
that has already been bootstrapped (need not be the first node)

**3.3.2 Gossip**

Gossip is a peer-to-peer communication protocol. Nodes discover
information about other nodes by exchanging state information about
themselves and other nodes they know about every second. This helps with
failure detection. Gossip information is also persisted locally by each
node to use immediately when a node restarts. 

**3.3.3 Partitioner**

A partitioner determines which node will receive the first replica of a
piece of data, and how to distribute other replicas across other nodes
in the cluster.A partitioner is a hash function that derives a token
from the primary key of a row. The default hash function used by
Cassandra is Murmur3. 

**3.3.4 Replica Placement**

A replication strategy determines which nodes to place replicas on. If
network topology is not input by the user, the nodes right to the
primary data in the ring will be used for replication. When network
topology is input, the replicas are placed in a such a way to increase
availability. 

**3.4 Consistency Levels**
============================

Since Cassandra is masterless a client can connect with any node in a
cluster. Clients can interface with a Cassandra node using either a
thrift protocol or using CQL. The node that a client connects to is
designated as the **coordinator** and is responsible for satisfying the
request. The **consistency level (CL)** determines the number of nodes
that the coordinator needs to hear from in order to notify the client of
a successful mutation. Consistency levels are tunable at runtime

**3.4.1 Default Consistency Level**

The default consistency level is 1. In case of a Write Operation, the
request is sent to all the replicas. However as soon as one of the node
acknowledge, the request is completed back to the client. In case of a
Read Operation, the request is sent to the best performing
node. Consistency Level 1 is the fastest but may not fetch the latest
data

**3.4.2 Consistency Level All**

In this mode, the co-ordinator will wait to hear from all the replicas
for READ and WRITE operations

**3.4.3 Consistency Level Quorum**

In this mode, the co-ordinator will wait to hear from majority of the
nodes. Here is the formula used 

Quorum = RoundDown(sum_of_replication_factors / 2) + 1

Where

sum_of_replication_factors = datacenter1_RF + . . . + datacenterN_RF

E.g. Using a replication factor of 3, a quorum is 2 nodes. The cluster
can tolerate 1 replica down.

The following consistency level gives immediate consistency

-   Write CL = ALL, Read CL = ONE

-   Write CL = ONE, Read CL = ALL

-   Write CL = QUORUM, Read CL = QUORUM

*4.0 A Day in the life of Cassandra*
---------------------------------------

Once the cluster is created, typical operations include creating the
record, modifying and reading the record, handling node failures and
expanding or shrinking the cluster. These are the typical Day 2
Operations performed on the cluster

**4.1 Write Operation**
=========================

Let us take the example of a user wanting to insert a record as below
using **cqlsh**

*INSERT INTO users(login,name,age) VALUES (\'jdoe\', \'John DOE\', 33);*

*Login (PK) -- jdoe*

*Name -- John DOE*

*Age - 33*

The co-ordinator receives the request and forward it to the Primary Node
as well as the replica Nodes. Each of the nodes act independently. The
node first writes the mutation to the commit log and then writes the
mutation to the memtable. The memtable is flushed to an immutable
structure called SSTable (let us say **sst1**) A timestamp (**t1**) is
autogenerated and is associated with the columns. By default every
SSTable creates three files on disk which include a bloom filter, a key
index and a data file.

**4.2 Update Operation**
==========================

Now let us assume Joe has turned 34 years old and we need to update a
record as below using **cqlsh**

*UPDATE users SET age = 34 WHERE login = \'jdoe\';*

*Login (PK) -- jdoe*

*Age - 33*

The SSTables are immutable, once written it cannot be updated. Cassandra
creates a new SSTable (let us say **sst2**) with timestamp of **t2**
with the above details.

**4.3 Compaction**
====================

Over a period of time a number of SSTables are created. This results in
the need to read multiple SSTables to satisfy a read request. E.g. in
the above scenario both sst1 and sst2 needs to be read for jdoe.
Compaction tries to solve it by eliminating multiple tables and creating
a new **sst3** that includes most recent version of each column from all
tables (sst2 and sst2) The old tables (sst1 and sst2) are deleted.

**4.4 Read**
==============

Now let us assume the user issues a following read command

*SELECT name, age from users WHERE login = \'jdoe\';*

To satisfy a read, Cassandra must combine results from the active
memtable and potentially multiple SSTables. As in the above example
after stage 4.2, the node has to read from both **sst1** and **sst2** to
return the result back to the co-ordinator. The co-ordinator based on
the consistency level configured returns the latest data back to the
client.

Reading data from the disk is costly. Hence Cassandra tries to find the
record in the memtable first. If the record is not found in memtable but
if the rowcache is enabled, it tries to return the data from the row
cache.

A unique aspect of Cassandra read is the implementation of bloom filter.
Bloom filter is a probabilistic function and each SSTable has
bloomfilter associated with it. A Bloom filter can establish that a
SSTable does not contain certain partition data. A Bloom filter can also
find the likelihood that partition data is stored in a SSTable. If the
bloom filter returns a negative response no data is returned from the
particular SSTable. This is  a common case as the compaction operation
tries to group all row key related data into as few SSTables as
possible. If the bloom filter provides a positive response the partition
key cache is scanned to ascertain the compression offset for the
requested row key. It then proceeds to fetch the compressed data on disk
and returns the result set. If the partition cache does not contain a
corresponding entry the partition key summary is scanned. The partition
summary is a subset to the partition index and helps determine the
approximate location of the index entry in the partition index. The
partition index is then scanned to locate the compression offset which
is then used to find the appropriate data on disk. 

**4.5 Failure detection and recovery**
=======================================

The gossip process tracks state from other nodes both directly (nodes
gossiping directly to it) and indirectly (nodes communicated about
second-hand, third-hand, and so on). During gossip exchanges, every node
maintains a sliding window of inter-arrival times of gossip messages
from other nodes in the cluster. Based on the threshold configured for
the failure detector, a node can be marked down if no response is
received within the stipulated time. Even after marking the node down,
other nodes will try to establish a connection with the downed host to
see if the node is back up. Only an administrator can remove the node
using the nodetool utility. When the node joins back, nodetool is used
to repair the node.
