---
layout: post
title: Notes from Hadoop Summit 2016
---

Having worked on Hadoop for 2 years, this is my first Hadoop Summit -- will be the only one as well, because the conference will be rebranded as *Dataworks Summit* next year. It took place in San Jose Convention Center from Tuesday 6/28 to Thursday 6/30. This post summarizes my key takeaways.

### Hadoop and Beyond
The rebranding as well as the conference [agenda](http://hadoopsummit.org/san-jose/agenda/) suggest a trend to focus more on the application layer (what can be done with Hadoop) instead of the low level technologies.

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
Although on-premise datacenters still run the lion's share of Hadoop deployments, it is an obvious trend to move big data workloads to public cloud.

HDFS and object storage talk from Hortonworks:

#### _General Improvements_

### SQL

### Streaming
