---
layout: post
title: My HDFS Research Agenda
---
Approaching its 10th or 4th year, depending on how you count it, and 9000 JIRA [issues](https://issues.apache.org/jira/browse/HDFS), HDFS is still a young project. Scores of interesting technical problems await solutions. 

HDFS touches many active research areas in the academia: operating system, data storage, distributed computing, just to name the major ones. More joint research projects between universities and companies would benefit both sides in unique ways, and advance the entire big data industry. 

Almost a year into HDFS development, below is my personal collection of open problems that should be studied more scientifically.

## Smart block data retention policy
HDFS already has a [Trash](http://www.cloudera.com/content/cloudera/en/documentation/cloudera-manager/v4-latest/Cloudera-Manager-Managing-Clusters/cmmc_hdfs_trash.html) mechanism to protect against mis-deletions on the file level. However, a block-level solution is desirable for [a few reasons](https://issues.apache.org/jira/browse/HDFS-8193). One fundamental problem is to sort out the priorities of blocks in the context of data retention. Existing caching eviction policies might not work well. 

## Storage policy / software defined storage
Heterogeneous storage management ([HSM](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-hdfs/ArchivalStorage.html)) was recently introduced to HDFS. While enhancing storage cost-efficiency, it requires system administrators to manually set the storage tier for each file. This burden scales with the amount of data in the system. A more advanced solution is follow the _software defined systems_

{% include twitter_plug.html %}