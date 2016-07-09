---
layout: post
title: Notes from Hadoop Summit 2016
---

Having worked on Hadoop for 2 years, this is my first Hadoop Summit -- will be the only one as well, because the conference will be rebranded as **Dataworks Summit** next year. It took place in San Jose Convention Center from Tuesday 6/28 to Thursday 6/30. This post summarizes my key takeaways.

### Hadoop and Beyond
The rebranding as well as the conference [agenda](http://hadoopsummit.org/san-jose/agenda/) suggest a trend to focus more on the application layer (what can be done with Hadoop) instead of the low level technologies.

Unfortunately I couldn't attend many of the application-focused talks (conference has 8~9 parallel sessions). I was impressed by the "[Unresonable effectiveness of ACID](https://youtu.be/agZHjjJFDPI?t=43m23s)" keynote talk from Microsoft -- ACID here stands for algorithms, cloud, IoT, and data. The talk highlights the **social aspect** of big data with an example of preventing school dropouts through large scale data collection and machine learning. Another keynote [talk](https://youtu.be/UZd3rFS93Mw?t=1h34m19s) by Ben Hammersley, my personal favorite from the conference, delivers a similar message in a humorous tune.

### HDFS and YARN
Above being said, a lot still remains to be done on Apache Hadoop, which has just turned 10 years old. From what I saw, scalability and cloud integration are the two focus areas this year.

#### _Scalability_
Apache Hadoop was initially designed with a single-master architecture. Many global-scale companies are deploying clusters with 5k to 10k nodes, and scalability is becoming a severe constraint.

Multi-DC HDFS talk from Twitter:
* Twitter's production Hadoop environment has multiple logical clusters in each DC
* Logical clusters are functionally-partitioned: ad hoc, stable production jobs, etc.
* Each logical cluster has 3 nameservices: ```/tmp```, ```/user```, ```/log```
* Each ```DataNode``` belongs to all 3 nameservices
* The replication protocol, ```Nfly```, is only used by jobs with small data volumes. It has a ```/nfly/``` prefix
* ```Nfly``` could leave orphan temporary files behind. They'll be cleaned up by retention program.

Small file analysis talk from Expedia:
* Built a tool, based on ```fsimage```, to detect and categorize small files
* Different approaches -- compaction, deletion, archival -- based on category

YARN federation talk from Microsoft:
* MSFT has made a lot of efforts in YARN and most of them are open source
* [YARN-2915](https://issues.apache.org/jira/browse/YARN-2915) tracks the effort
* Target is 100k nodes
* MSFT has several 50k-node clusters, and run many short-lived jobs (a few seconds)
* Federation is not only for scalability, but also for cross-DC queries
* Architecture is based on ```Router``` + ```StateStore```
* Every node has a ```AM-RM-Proxy```
* Different routing policies can be used, leading to interesting trade-offs

YARN timeline service v2 talk from Twitter
* TLS v1 has a single server and single LevelDB
* TLS v2 uses HBase, has metrics aggregation, and offers richer query APIs

#### _Cloud_
Although on-premise datacenters still run the lion's share of Hadoop deployments, it is an obvious trend to move big data workloads to public or private cloud platforms. ```HDInsight``` was mentioned a lot.

HDFS tiered storage talk from Microsoft (AKA Tachyon-done-right):
* Emphasizes the problem of multiple clusters (even before moving to cloud)
* Compared to other approaches (```DistCp```, application-manage multi-DC access), transparency is big win
* The community has long discussed approaches to "stage" or "page" part of the data / metadata to external store
* The key idea here is to generalize the block concept and introduce a ```PROVIDED``` block type
* ```DataNode``` will run daemon for the actual data transfer
* When used together with ```HDInsight```, computation and data "jobs" will be co-scheduled to achieve just-in-time data staging
* Many smart algorithms can be considered for eviction and prediction-based prefetching

HDFS and object storage talk from Hortonworks:
* Interesting summary of different usage patterns of Hadoop on cloud: 1) all I/O happens to HDFS, and HDFS stages data with blob store; 2) input data from blob store, output data written to HDFS and eventually copied to blob store; 3) both input and output I/O on blob store, HDFS only has temporary data.
* Interesting summary of pros and cons of blob store and HDFS

Operationalizing YARN in the cloud talk from Qubole: [to be added]

#### _General Improvements_

Over-committing YARN resources talk from Yahoo:
* A lot of jobs are poorly configured, causing resources wastage
* Static overcommit (configure containers to use more than OS offers) doesn't work just like "over-selling flight tickets"
* ```NodeManager``` reports utlization to ```ResourceManager``` via heartbeats, to facilitate dynamic overcommitting

### SQL

### Streaming

