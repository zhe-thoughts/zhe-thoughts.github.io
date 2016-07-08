---
layout: post
title: Notes from Hadoop Summit 2016
---

Having worked on Hadoop for 2 years, this is my first Hadoop Summit -- will be the only one as well, because the conference will be rebranded as *Dataworks Summit* next year. It took place in San Jose Convention Center from Tuesday 6/28 to Thursday 6/30. This post summarizes my key takeaways.

#### Hadoop and Beyond
The rebranding as well as the conference [agenda](http://hadoopsummit.org/san-jose/agenda/) suggest a trend to focus more on the application layer (what can be done with Hadoop) instead of the low level technologies.

### HDFS and YARN
Above being said, a lot still remains to be done on Apache Hadoop, which has just turned 10 years old. From what I saw, scalability and cloud integration are the two focus areas this year.

#### _Scalability_
Apache Hadoop was initially designed with a single-master architecture. Many global-scale companies are deploying clusters with 5k to 10k nodes, and scalability is becoming a severe constraint.

YARN federation from Microsoft:
* [YARN-2915](https://issues.apache.org/jira/browse/YARN-2915) tracks the effort
* Target is 100k nodes
* MSFT has several 50k-node clusters, and run many short-lived jobs
* 

Multi-DC HDFS from Twitter:

#### _Cloud_

### SQL

### Streaming
